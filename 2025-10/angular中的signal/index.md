## 声明顺序
* react中的useState必须在组件的最顶层声明，不能放在条件语句、循环语句、函数内等位置，以免改变声明的顺序。
* angular中的signal没有顺序要求。

## 变更检测机制
* signal完全不依赖于angular的zone.js，而是类似于react的useState机制，当调用signal的set方法时，会触发组件的更新。
* angular的signal与react的useState不同之处
  * useState当一个state变化时，会导致整个组件进行重新渲染，包括所有的子组件，所以如果整个组件含有很多子组件，每次state变化都会触发大量的组件渲染，效率较低，需要使用React.memo来进行优化。
  * signal有更细的力度，他只会更新被signal影响的那一部分DOM，比如
    ```html
    <h2>Child B</h2>
    <div>Parent Signal Value: {{ dataService.mySignal() }}</div>
    <hr>
    <grandchild-c></grandchild-c>
    ```
    这里mySignal变化时grandchild-c不会重新渲染，而是只更新div中的值。

## 使用immer更新object
* 类似react.js中的useImmer, angular的signal也提供了immer的更新机制。示例如下：
  ```typescript
  import { WritableSignal } from '@angular/core';
  import { produce, Draft } from 'immer';

  // 一个通用的 update 函数，它接收一个 Signal 和一个 immer recipe
  export function updateWithImmer<T>(
    signal: WritableSignal<T>,
    recipe: (draft: Draft<T>) => void
  ) {
    signal.update(currentState => produce(currentState, recipe));
  }

  // 在组件中这样使用：
  import { updateWithImmer } from './immer-update.helper';

  // ...
  export class TodoListComponent {
    // ...
    toggleTodo(todoId: number) {
      updateWithImmer(this.todos, draft => {
        const todoToToggle = draft.find(todo => todo.id === todoId);
        if (todoToToggle) {
          todoToToggle.completed = !todoToToggle.completed;
        }
      });
    }
  }
  ```

## 使用useReducer
* angular的signal使用ngrx库来带到useRducer的效果
  ```typescript
  // 1. Define the shape of your state
  export interface UserProfileState {
    user: {
      id: number | null;
      username: string;
      email: string;
    };
    isLoading: boolean;
    error: string | null;
  }

  // 2. Define the initial state
  const initialState: UserProfileState = {
    user: {
      id: null,
      username: 'Guest',
      email: '',
    },
    isLoading: false,
    error: null,
  };
  ```
  ```typescript
  import { signalStore, withState, withMethods, patchState } from '@ngrx/signals';
  import { inject } from '@angular/core';
  import { UserService } from './user.service'; // A hypothetical service to fetch data

  // ... (State interface and initial state from above) ...

  export const UserProfileStore = signalStore(
    { providedIn: 'root' },
    withState(initialState),
    // Add methods to interact with the state
    withMethods((store, userService = inject(UserService)) => ({
      // Method to update just one part of a nested object
      updateUsername(newUsername: string) {
        // IMPORTANT: For nested objects, you must spread the existing properties
        // to avoid losing them.
        const user = { ...store.user(), username: newUsername };
        patchState(store, { user });
      },

      // Method to handle an async operation (fetching a user)
      async loadUser(id: number) {
        // Update multiple properties at once
        patchState(store, { isLoading: true, error: null });

        try {
          const fetchedUser = await userService.fetchUserById(id);
          // On success, update multiple properties again
          patchState(store, {
            user: fetchedUser,
            isLoading: false,
          });
        } catch (e) {
          // On error, update the error and loading state
          patchState(store, {
            error: 'Failed to fetch user.',
            isLoading: false,
          });
        }
      },
    })),
    // Add computed signals
    withComputed((store) => ({
      // A simple computed signal from one property
      isGuest: computed(() => store.user().id() === null),
      // A more complex "view model" signal, combining multiple properties
      // This is a very powerful pattern for components
      vm: computed(() => ({
        user: store.user(),
        isLoading: store.isLoading(),
        error: store.error(),
      })),
    }))
  );
  ```
  ```typescript
  @Component({
    selector: 'app-profile',
    template: `
      <div *ngIf="store.vm().isLoading">Loading...</div>
      <div *ngIf="store.vm().error" class="error">{{ store.vm().error }}</div>

      <div *ngIf="!store.vm().isLoading && !store.vm().error">
        <h2>Welcome, {{ store.vm().user.username() }}!</h2>
        <p>Is Guest: {{ store.isGuest() }}</p>

        <input #usernameInput [value]="store.vm().user.username()" />
        <button (click)="store.updateUsername(usernameInput.value)">Update Name</button>
        <button (click)="store.loadUser(1)">Load User 1</button>
      </div>
    `,
  })
  export class ProfileComponent {
    protected readonly store = inject(UserProfileStore);
  }
  ```

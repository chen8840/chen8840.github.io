### Java里面String的编码问题
Java里面内置字符串全部是utf-16编码，详细的编码方式[看这里](http://www.ruanyifeng.com/blog/2014/12/unicode.html)
```JAVA
import java.nio.charset.Charset;
import java.util.Arrays;
import java.util.Map;

public class String2Bytes {
    public static void main(String[] args) {
        String str = "\u005Bab我";
        Map<String, Charset> charsetMap = Charset.availableCharsets();
        for(String charsetName : charsetMap.keySet()) {
            System.out.println(charsetName + ":" + charsetMap.get(charsetName));
        }
        System.out.println(str.charAt(3));
        //String的getBytes()方法是得到一个字串的字节数组，这是众所周知的。
        //但特别要注意的是，本方法将返回该操作系统默认的编码格式的字节数组。
        //如果你在使用这个方法时不考虑到这一点，你会发现在一个平台上运行良好的系统，放到另外一台机器后会产生意想不到的问题。
        System.out.println(Arrays.toString(str.getBytes()));

        //附加级别的字符
        char[] c = Character.toChars(Integer.parseInt("1D306", 16));
        String str1 = new String(c);
        System.out.println(str1);
        //附加级别的字符占用了2个代码单元
        System.out.println(c.length);
        System.out.println(str1.length());
        //但是只占用一个代码点
        System.out.println(str1.codePointCount(0, str1.length()));

        //访问第一个代码单元，这种做法是错误的
        System.out.println(str1.charAt(0));

        //查看保存在内存里的方式
        /*
         * 算法
         * H = Math.floor((c-0x10000) / 0x400)+0xD800
         * L = (c - 0x10000) % 0x400 + 0xDC00
         */
        System.out.println(Integer.toString((int)str1.charAt(0), 16));//0xD834
        System.out.println(Integer.toString((int)str1.charAt(1), 16));//0xDF06

        //访问第一个代码点，正确的做法
        int index = str1.offsetByCodePoints(0, 0);
        System.out.println(str1.codePointAt(index));

        String str2 = str1 + "abc";
        //顺序访问一个String正确的方式
        int count = str2.codePointCount(0, str2.length());
        for(int i=0; i < count; ++i) {
            int index1 = str2.offsetByCodePoints(0, i);
            int cp = str2.codePointAt(index1);
            System.out.print(Character.toChars(cp));
        }
        System.out.println();
    }
}
```

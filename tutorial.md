# VsCode插件

- Chinese (Simplified) (简体中文) Language Pack for Visual Studio Code
- Code Spell Checker  （单词拼写检查）
- CSS Peek   （CSS路径追踪）
- HTML CSS Support  （CSS）
- Image Preview  （图片预览）
- JavaScript(E65) code snippets  （JS）
- Live Server （网页实时预览）
- Prettier-Code formatter  （格式化）
- code-translator  （翻译工具）
- CodeSnap （代码截图）
- Sublime Text KeyMap and Setting Importer （sublime 快捷键）
- Code Runner （运行插件）

# C++运行脚本

```shell
#!/bin/bash

target="$1"
src="${target}.cpp"

g++ -std=c++23 -O2 -Wall -Wextra "$src" -o "$target"

./"$target" < ${target}.txt > output.txt
```

# C++

g++解释器[msys2](https://www.msys2.org/)

# Python

pypy解释器:[pypy](https://pypy.org/)

# 梯子

[最萌的云](https://main.cute-cloud.de/)

[Clash for Windows](https://www.clashforwindows.net/)

# sublime

### 插件

[CppFastOlympicCoding](https://github.com/Jatana/FastOlympicCoding)

[competitive-companion](https://github.com/jmerle/competitive-companion)

[Hook](https://github.com/DrSwad/FastOlympicCodingHook?tab=readme-ov-file)

把压缩包解压完放到`Packages`里

代码片段（模板）

```
<snippet>
    <content><![CDATA[
#include <bits/stdc++.h>

using i64 = long long;

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(0);

    ${0}


    return 0;
}
]]></content>
    <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
    <tabTrigger>acm</tabTrigger> 
    <!-- 这个是触发词，输入acm就可以按下tab 或者 enter 触发 -->
    <!-- Optional: Set a scope to limit where the snippet will trigger -->
    <!-- <scope>source.cpp</scope> -->
</snippet>
```

字体（不含斜体）

```
{
    "name": "Monokai",
    "author": "Sublime HQ Pty Ltd, Wimer Hazenberg",
    "variables":
    {
        "black": "hsl(0, 0%, 0%)",
        "black2": "hsl(60, 17%, 11%)",
        "black3": "hsl(70, 8%, 15%)",
        "blue": "hsl(190, 81%, 67%)",
        "grey": "hsla(55, 8%, 31%, 0.7)",
        "orange": "hsl(32, 98%, 56%)",
        "orange2": "hsl(30, 83%, 34%)",
        "orange3": "hsl(47, 100%, 79%)",
        "purple": "hsl(261, 100%, 75%)",
        "red": "hsl(0, 93%, 59%)",
        "red2": "hsl(338, 95%, 56%)",
        "white": "hsl(0, 0%, 97%)",
        "white2": "hsl(60, 36%, 96%)",
        "white3": "hsl(60, 30%, 96%)",
        "yellow": "hsl(54, 70%, 68%)",
        "yellow2": "hsl(80, 76%, 53%)",
        "yellow3": "hsl(60, 12%, 79%)",
        "yellow4": "hsl(55, 11%, 22%)",
        "yellow5": "hsl(50, 11%, 41%)"
    },
    "globals":
    {
        "foreground": "var(white3)",
        "background": "var(black3)",
        "caret": "color(var(white2) alpha(0.9))",
        "block_caret": "color(var(white2) alpha(0.2))",
        "block_caret_border": "color(var(white2) alpha(0.8))",
        "invisibles": "color(var(white3) alpha(0.35))",
        "line_highlight": "var(yellow4)",
        "selection": "var(grey)",
        "selection_border": "var(black2)",
        "misspelling": "var(red2)",
        "active_guide": "color(var(orange2) alpha(0.69))",
        "find_highlight_foreground": "var(black)",
        "find_highlight": "var(orange3)",
        "brackets_options": "underline",
        "brackets_foreground": "color(var(white3) alpha(0.65))",
        "bracket_contents_options": "underline",
        "bracket_contents_foreground": "color(var(white3) alpha(0.65))",
        "tags_options": "stippled_underline",
    },
    "rules":
    [
        {
            "name": "Comment",
            "scope": "comment",
            "foreground": "var(yellow5)"
        },
        {
            "name": "String",
            "scope": "string",
            "foreground": "var(yellow)"
        },
        {
            "name": "Number",
            "scope": "constant.numeric",
            "foreground": "var(purple)"
        },
        {
            "name": "Number suffix",
            "scope": "storage.type.numeric",
            "foreground": "var(blue)",
            "font_style": ""
        },
        {
            "name": "Built-in constant",
            "scope": "constant.language",
            "foreground": "var(purple)"
        },
        {
            "name": "User-defined constant",
            "scope": "constant.character, constant.other",
            "foreground": "var(purple)"
        },
        {
            "name": "Variable",
            "scope": "variable"
        },
        {
            "name": "Keyword",
            "scope": "keyword - (source.c keyword.operator | source.c++ keyword.operator | source.objc keyword.operator | source.objc++ keyword.operator), keyword.operator.word, source.ruby keyword.declaration",
            "foreground": "var(red2)"
        },
        {
            "name": "Annotation Punctuation",
            "scope": "punctuation.definition.annotation",
            "foreground": "var(red2)"
        },
        {
            "name": "JavaScript Dollar",
            "scope": "variable.other.dollar.only.js",
            "foreground": "var(red2)"
        },
        {
            "name": "Storage",
            "scope": "storage",
            "foreground": "var(red2)"
        },
        {
            "name": "Storage type",
            "scope": "storage.type",
            "foreground": "var(blue)",
            "font_style": ""
        },
        {
            "name": "Entity name",
            "scope": "entity.name - (entity.name.filename | entity.name.section | entity.name.tag | entity.name.label)",
            "foreground": "var(yellow2)"
        },
        {
            "name": "Inherited class",
            "scope": "entity.other.inherited-class",
            "foreground": "var(yellow2)",
            "font_style": ""
        },
        {
            "name": "Function argument",
            "scope": "variable.parameter - (source.c | source.c++ | source.objc | source.objc++)",
            "foreground": "var(orange)",
            "font_style": ""
        },
        {
            "name": "Language variable",
            "scope": "variable.language",
            "foreground": "var(orange)",
            "font_style": ""
        },
        {
            "name": "Tag name",
            "scope": "entity.name.tag | meta.mapping.key string.unquoted",
            "foreground": "var(red2)"
        },
        {
            "name": "Tag attribute",
            "scope": "entity.other.attribute-name",
            "foreground": "var(yellow2)"
        },
        {
            "name": "Function call",
            "scope": "variable.function, variable.annotation",
            "foreground": "var(blue)"
        },
        {
            "name": "Library function",
            "scope": "support.function, support.macro",
            "foreground": "var(blue)"
        },
        {
            "name": "Library constant",
            "scope": "support.constant",
            "foreground": "var(blue)"
        },
        {
            "name": "Library class/type",
            "scope": "support.type, support.class",
            "foreground": "var(blue)",
            "font_style": ""
        },
        {
            "name": "Library variable",
            "scope": "support.other.variable"
        },
        {
            "name": "Invalid",
            "scope": "invalid",
            "foreground": "var(white2)",
            "background": "var(red2)"
        },
        {
            "name": "Invalid deprecated",
            "scope": "invalid.deprecated",
            "foreground": "var(white2)",
            "background": "var(purple)"
        },
        {
            "name": "JSON String",
            "scope": "meta.structure.dictionary.json string.quoted.double.json",
            "foreground": "var(yellow3)"
        },
        {
            "name": "YAML String",
            "scope": "string.unquoted.yaml",
            "foreground": "var(white3)"
        },
        {
            "name": "diff.header",
            "scope": "meta.diff, meta.diff.header",
            "foreground": "var(yellow5)"
        },
        {
            "name": "markup headings",
            "scope": "markup.heading",
            "font_style": "bold"
        },
        {
            "name": "markup headings",
            "scope": "markup.heading punctuation.definition.heading",
            "foreground": "var(orange)"
        },
        {
            "name": "markup h1",
            "scope": "markup.heading.1 punctuation.definition.heading",
            "foreground": "var(red2)"
        },
        {
            "name": "markup links",
            "scope": "markup.underline.link",
            "foreground": "var(blue)"
        },
        {
            "name": "markup bold",
            "scope": "markup.bold",
            "font_style": "bold"
        },
        {
            "name": "markup italic",
            "scope": "markup.italic",
            "font_style": ""
        },
        {
            "name": "markup underline",
            "scope": "markup.underline",
            "font_style": "underline"
        },
        {
            "name": "markup bold/italic",
            "scope": "markup.italic markup.bold | markup.bold markup.italic",
            "font_style": "bold italic"
        },
        {
            "name": "markup bold/underline",
            "scope": "markup.underline markup.bold | markup.bold markup.underline",
            "font_style": "bold underline"
        },
        {
            "name": "markup italic/underline",
            "scope": "markup.underline markup.italic | markup.italic markup.underline",
            "font_style": ""
        },
        {
            "name": "markup bold/italic/underline",
            "scope": "markup.bold markup.italic markup.underline | markup.bold markup.underline markup.italic | markup.italic markup.bold markup.underline | markup.italic markup.underline markup.bold | markup.underline markup.bold markup.italic | markup.underline markup.italic markup.bold",
            "font_style": ""
        },
        {
            "name": "markup hr",
            "scope": "punctuation.definition.thematic-break",
            "foreground": "var(yellow5)"
        },
        {
            "name": "markup blockquote",
            "scope": "markup.quote punctuation.definition.blockquote",
            "foreground": "var(yellow5)"
        },
        {
            "name": "markup bullets",
            "scope": "markup.list.numbered.bullet",
            "foreground": "var(purple)"
        },
        {
            "name": "markup bullets",
            "scope": "markup.list.unnumbered.bullet | (markup.list.numbered punctuation.definition)",
            "foreground": "color(var(white) alpha(0.67))"
        },
        {
            "name": "markup code",
            "scope": "markup.raw",
            "background": "color(var(white) alpha(0.094))"
        },
        {
            "name": "markup punctuation",
            "scope": "markup.raw punctuation.definition.raw",
            "foreground": "color(var(white) alpha(0.67))"
        },
        {
            "name": "markup punctuation",
            "scope": "text & (punctuation.definition.italic | punctuation.definition.bold | punctuation.definition.raw | punctuation.definition.link | punctuation.definition.metadata | punctuation.definition.image | punctuation.separator.table-cell | punctuation.section.table-header | punctuation.definition.constant)",
            "foreground": "color(var(white) alpha(0.67))"
        },
        {
            "name": "diff.deleted",
            "scope": "markup.deleted",
            "foreground": "var(red2)"
        },
        {
            "name": "diff.inserted",
            "scope": "markup.inserted",
            "foreground": "var(yellow2)"
        },
        {
            "name": "diff.changed",
            "scope": "markup.changed",
            "foreground": "var(yellow)"
        },
        {
            "scope": "constant.numeric.line-number.find-in-files - match",
            "foreground": "color(var(purple) alpha(0.63))"
        },
        {
            "scope": "entity.name.filename",
            "foreground": "var(yellow)"
        },
        {
            "scope": "message.error",
            "foreground": "var(red)"
        },

        {
            "scope": "diff.deleted",
            "background": "hsla(338, 50%, 56%, 0.15)",
            "foreground_adjust": "l(+ 5%)"
        },
        {
            "scope": "diff.deleted.char",
            "background": "hsla(338, 65%, 56%, 0.30)",
            "foreground_adjust": "l(+ 10%)"
        },
        {
            "scope": "diff.inserted",
            "background": "hsla(80, 50%, 53%, 0.15)",
            "foreground_adjust": "l(+ 5%)"
        },
        {
            "scope": "diff.inserted.char",
            "background": "hsla(80, 65%, 53%, 0.30)",
            "foreground_adjust": "l(+ 10%)"
        },
    ]
}

```

# Fira Code

[字体](https://github.com/tonsky/FiraCode)

# 油猴

[Tampermonkey](https://www.tampermonkey.net/index.php?locale=zh)

- acwing-helper
- AtCode Better
- CodeForces Better
- Emojiforce
- Github汉化插件
- Nowcoder Better
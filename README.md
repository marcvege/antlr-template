# antlr-template
[Antlr](https://www.antlr.org/) is  (ANother Tool for Language Recognition) is a powerful parser generator for reading, processing, executing, or translating structured text or binary files. It's widely used to build languages, tools, and frameworks. From a grammar, ANTLR generates a parser that can build and walk parse trees.

This project is just a **small template** to start working on it.

## Grammar
Antlr allows us to specify a grammar in an [extended Backus–Naur](https://tomassetti.me/ebnf/) form (EBNF)

The grammar is stored in a `.g4` file in the following format:
~~~g4
grammar MyG;
rule1 : «stuff»; 
rule2 : «more stuff»; 
...
~~~

_Note: The grammar name need to match with the file name_

In this [repository](https://github.com/antlr/grammars-v4) it is available a list of grammar examples:
 * [Json](https://github.com/antlr/grammars-v4/tree/master/json) based on [json grammar specification](https://www.json.org/json-en.html)
 * [Kotlin](https://github.com/antlr/grammars-v4/tree/master/kotlin)
 * [HTTP](https://github.com/antlr/grammars-v4/blob/master/http/http.g4)

💡 Install the [ANTLR v4 plugin](https://plugins.jetbrains.com/plugin/7358-antlr-v4) allows to visualise the parsing a text using a grammar, it displays errors if the text is not correct or shows the tree of the parsed text.
See more [tools for other IDE](https://www.antlr.org/tools.html)

## Generate Grammar source
Given a `.g4` file containing a well-formed grammar we will be able to generate java for process the grammar using this gradle task: 
~~~bash
./gradlew generateGrammarSource
~~~
Notes:
* By default, the code is generated in a `/gen` directory. It's possible configure the output directory in the `build.gradle`:
    ~~~groovy
    generateGrammarSource {
        outputDirectory = file("src/main/java/org/antlr/template/parser")
    }
    ~~~
* By default, the classes generated don't have a package defined. It's possible define a package in th `.g4` file adding:
    ~~~g4
    @header {
        package org.antlr.template.parser;
    }
    ~~~

## Integrate a Generated Parser into your code
~~~kotlin
import org.antlr.v4.runtime.CharStreams
import org.antlr.v4.runtime.CommonTokenStream
import org.antlr.v4.runtime.tree.ParseTree

fun main() {
    // create a CharStream that reads from standard input
    val input = CharStreams.fromStream("text to parse".byteInputStream())
    // create a lexer that feeds off of input CharStream
    val lexer = [GrammarName]Lexer(input)
    // create a buffer of tokens pulled from the lexer
    val tokens = CommonTokenStream(lexer)
    // create a parser that feeds off the tokens buffer
    val parser = [GrammarName]Parser(tokens)
    // begin parsing at specific rule
    val tree: ParseTree = parser.[ruleName]()
    // print LISP-style tree 
    println(tree.toStringTree(parser)) 
}
~~~
where:
* `[GrammarName]` is the name of the grammar specified in the `.g4` file.
* `[ruleName]` is the mame of the main rule defined in the `.g4` file.

## Related links pending to review
* https://github.com/bkiers/rrd-antlr4 - Generate grammar diagram specification

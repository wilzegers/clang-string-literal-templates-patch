# Clang string literal templates patch

Using this patch, Clang can interpret string literal template arguments as char parameter packs.
For example: ```String<"example">``` will be interpeted as ```String<'e','x','a','m','p','l','e'>```.

## Compiling using the feature
To compile using the feature, use the flags ```-Xclang -fstring-literal-templates```.

Compiling the example: ``` clang++ -std=c++11 -ftemplate-depth=123456789 -Xclang -fstring-literal-templates example.cpp ```

## Measurement results

I modified the boost.metaparse library to use this feature to construct its compile-time string, ``` BOOST_METAPARSE_STRING ```, if it's supported. The following measurements compare the compile time of the following three string constructions (these are just examples):
* Generating strings manually: ```boost::metaparse::string<'e','x','a','m','p','l','e'>```
* Using BOOST_METAPARSE_STRING: ```BOOST_METAPARSE_STRING("example")```- without switching the feature on
* Using BOOST_METAPARSE_STRING with string literal templates: ```BOOST_METAPARSE_STRING("example")```- using the feature

### Instantiating 128 strings of the given length
![](https://github.com/wilzegers/clang-string-literal-templates-patch/blob/master/length128_clang_4.0.0.png)

### Instantiating the given number of 64 character long strings
![](https://github.com/wilzegers/clang-string-literal-templates-patch/blob/master/number_clang_4.0.0.png)

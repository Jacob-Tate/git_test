# Tokenization

When we want to preform any substring algorithms we will want to remove things from the code which are not needed for the algroithm we will refer to this process as _pre-processing_ 

## Stages

{% tabs %}
{% tab title="Code" %}

{% endtab %}

{% tab title="Parser" %}

{% endtab %}

{% tab title="In-memory data structure" %}

{% endtab %}

{% tab title="Normalizer" %}

{% endtab %}

{% tab title="Normalized code" %}

{% endtab %}
{% endtabs %}

### Parsing

During this stage we will need to parse the code into a grammarless context which can then be used by the algorithm

#### Function declarations

{% tabs %}
{% tab title="Original" %}
```cpp
int addition(int lhs, int rhs)
```
{% endtab %}

{% tab title="Parsed" %}
```text
FunctionDecl 0x2953788 <file.c:1:1, line:6:1> line:1:5 addition 'int (int, int)'
|-ParmVarDecl 0x28fa548 <col:15, col:19> col:19 used lhs 'int'
|-ParmVarDecl 0x29536b0 <col:24, col:28> col:28 used rhs 'int'
```
{% endtab %}
{% endtabs %}

#### TODO FINISH THIS PART

#### Example

{% tabs %}
{% tab title="Original" %}
{% code title="test.c" %}
```cpp
int addition1(int num1, int num2)
{
    int sum;
    sum = num1 + num2;

    return sum;
}

int addition2(int num1, int num2)
{
    return num1 + num2;
}

int main()
{
    return 0;
}
```
{% endcode %}
{% endtab %}

{% tab title="Parsed" %}
{% code title="test.parsed" %}
```text
TranslationUnitDecl 0x28f98b8 <<invalid sloc>> <invalid sloc>
|-TypedefDecl 0x28f9e30 <<invalid sloc>> <invalid sloc> implicit __int128_t '__int128'
| `-BuiltinType 0x28f9b50 '__int128'
|-TypedefDecl 0x28f9ea0 <<invalid sloc>> <invalid sloc> implicit __uint128_t 'unsigned __int128'
| `-BuiltinType 0x28f9b70 'unsigned __int128'
|-TypedefDecl 0x28fa178 <<invalid sloc>> <invalid sloc> implicit __NSConstantString 'struct __NSConstantString_tag'     | `-RecordType 0x28f9f80 'struct __NSConstantString_tag'
|   `-Record 0x28f9ef8 '__NSConstantString_tag'
|-TypedefDecl 0x28fa210 <<invalid sloc>> <invalid sloc> implicit __builtin_ms_va_list 'char *'
| `-PointerType 0x28fa1d0 'char *'
|   `-BuiltinType 0x28f9950 'char'
|-TypedefDecl 0x28fa4d8 <<invalid sloc>> <invalid sloc> implicit __builtin_va_list 'struct __va_list_tag [1]'
| `-ConstantArrayType 0x28fa480 'struct __va_list_tag [1]' 1
|   `-RecordType 0x28fa2f0 'struct __va_list_tag'
|     `-Record 0x28fa268 '__va_list_tag'
|-FunctionDecl 0x2953788 <test.c:1:1, line:6:1> line:1:5 addition1 'int (int, int)'
| |-ParmVarDecl 0x28fa548 <col:15, col:19> col:19 used lhs 'int'
| |-ParmVarDecl 0x29536b0 <col:24, col:28> col:28 used rhs 'int'
| `-CompoundStmt 0x2953a58 <line:2:1, line:6:1>
|   |-DeclStmt 0x29538f0 <line:3:2, col:9>
|   | `-VarDecl 0x2953890 <col:2, col:6> col:6 used sum 'int'
|   |-BinaryOperator 0x29539d8 <line:4:2, col:14> 'int' '='
|   | |-DeclRefExpr 0x2953908 <col:2> 'int' lvalue Var 0x2953890 'sum' 'int'
|   | `-BinaryOperator 0x29539b0 <col:8, col:14> 'int' '+'
|   |   |-ImplicitCastExpr 0x2953980 <col:8> 'int' <LValueToRValue>
|   |   | `-DeclRefExpr 0x2953930 <col:8> 'int' lvalue ParmVar 0x28fa548 'lhs' 'int'
|   |   `-ImplicitCastExpr 0x2953998 <col:14> 'int' <LValueToRValue>
|   |     `-DeclRefExpr 0x2953958 <col:14> 'int' lvalue ParmVar 0x29536b0 'rhs' 'int'
|   `-ReturnStmt 0x2953a40 <line:5:2, col:9>
|     `-ImplicitCastExpr 0x2953a28 <col:9> 'int' <LValueToRValue>
|       `-DeclRefExpr 0x2953a00 <col:9> 'int' lvalue Var 0x2953890 'sum' 'int'
|-FunctionDecl 0x2953ba8 <line:8:1, line:11:1> line:8:5 addition2 'int (int, int)'
| |-ParmVarDecl 0x2953a98 <col:15, col:19> col:19 used lhs 'int'
| |-ParmVarDecl 0x2953b10 <col:24, col:28> col:28 used rhs 'int'
| `-CompoundStmt 0x2953d18 <line:9:1, line:11:1>
|   `-ReturnStmt 0x2953d00 <line:10:2, col:15>
|     `-BinaryOperator 0x2953cd8 <col:9, col:15> 'int' '+'
|       |-ImplicitCastExpr 0x2953ca8 <col:9> 'int' <LValueToRValue>
|       | `-DeclRefExpr 0x2953c58 <col:9> 'int' lvalue ParmVar 0x2953a98 'lhs' 'int'
|       `-ImplicitCastExpr 0x2953cc0 <col:15> 'int' <LValueToRValue>
|         `-DeclRefExpr 0x2953c80 <col:15> 'int' lvalue ParmVar 0x2953b10 'rhs' 'int'
`-FunctionDecl 0x2953d80 <line:13:1, line:16:1> line:13:5 main 'int ()'
  `-CompoundStmt 0x2953e58 <line:14:1, line:16:1>
    `-ReturnStmt 0x2953e40 <line:15:2, col:9>
      `-IntegerLiteral 0x2953e20 <col:9> 'int' 0
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Normalizing

During pre-processing we will attempt to normalize the code some of these normalizations include

#### Normalizing Loops

{% tabs %}
{% tab title="Original" %}
```cpp
for(exp1;exp2;exp3)
    body-of-loop
```
{% endtab %}

{% tab title="Normalized" %}
```cpp
exp1;
while(true)
{
    if(!exp2) {break;}
    body-of-loop
    exp3;
}
```
{% endtab %}
{% endtabs %}

#### Only two logical operators: ! and &&

{% tabs %}
{% tab title="Original" %}
```cpp
x || y
```
{% endtab %}

{% tab title="Normalized" %}
```cpp
! ( ! x && ! y)
```
{% endtab %}
{% endtabs %}

#### Only two bitwise operators: ~ and &

{% tabs %}
{% tab title="Original" %}
```cpp
x | y
x ^ y
```
{% endtab %}

{% tab title="Normalized" %}
```cpp
~( ~x & ~y )
~( ~( ~x & y ) & ~( x & ~y ) )
```
{% endtab %}
{% endtabs %}

#### Only two relational operators: &lt; and ==

{% tabs %}
{% tab title="Original" %}
```cpp
x > y
x <= y
x >= y
x != y
```
{% endtab %}

{% tab title="Normalized" %}
```cpp
y < x
x < y || x == y
y < x || x == y
! ( x == y)
```
{% endtab %}
{% endtabs %}

#### No multiple assignments in the same statement

{% tabs %}
{% tab title="Original" %}
```cpp
x += y = a + b + c, z = n = foo();
```
{% endtab %}

{% tab title="Normalized" %}
```cpp
y = a + b + c;
x += y;
n = foo();
z = n;
```
{% endtab %}
{% endtabs %}

#### Merge successive assignments with the same lvalue

{% tabs %}
{% tab title="Original" %}
```cpp
x = a;
x += b;
x *= c;
```
{% endtab %}

{% tab title="Normalized" %}
```cpp
x = (a + b) * c;
```
{% endtab %}
{% endtabs %}

#### The condition of an if-statement cannot be a negated expression

{% tabs %}
{% tab title="Original" %}
```cpp
if ( !condition )
    block_1
else
    block_2
```
{% endtab %}

{% tab title="Normalized" %}
```cpp
if ( condition )
    block_2
else
    block_1
```
{% endtab %}
{% endtabs %}

#### Only one assignment operator: =

{% tabs %}
{% tab title="Original" %}
```cpp
x += y
x >>= y
```
{% endtab %}

{% tab title="Normalized" %}
```cpp
x = x + y
x = x >> y
```
{% endtab %}
{% endtabs %}

#### Distribute multiplication over addition whenever possible

{% tabs %}
{% tab title="Original" %}
```cpp
x * (y - z) / k
```
{% endtab %}

{% tab title="Normalized" %}
```cpp
x * y / k - x * z / k
```
{% endtab %}
{% endtabs %}






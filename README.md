Download Link: https://assignmentchef.com/product/solved-ece39595-homework-8
<br>
You should convert the supplied program into three forms:

<ol>

 <li>A program that only uses unique_ptr</li>

 <li>A program that only uses shared_ptr and does initialization using std::shared_ptr&lt;Type1&gt;(new Type2(<em>args</em>))</li>

</ol>

where Type2 ISA Type1

<ol start="3">

 <li>A program that only uses shared_ptr and does initialization using std::make_shared&lt;Type1&gt;(Type2(…))</li>

</ol>

where Type1 is not abstract and Type2 ISA Type1. More on why Type1 should not be abstract below under “Some useful information”.

<strong>Some useful information. </strong>

<em>First bit of useful information: </em>

When you have a statement that looks like an assignment but is doing an initialization, e.g.,

Type1 var = std::move(unique_ptr&lt;Type1&gt;(new Type2(<em>args</em>)));

or

var = std::move(unique_ptr&lt;Type1&gt;(new Type2(<em>args</em>)));

or

Type1( ) : var(std::move(unique_ptr&lt;Type1&gt;(new Type2(<em>args</em>)))) {. . .}

where var is an object field and the statement is an initialization being performed in a constructor, C++ is actually doing an initialization and not an assignment. In these cases you do not have to use std::move(…), and the statements above could be correctly written as: Type1 var = unique_ptr&lt;Type2&gt;(new Type2(<em>args</em>)); var = unique_ptr&lt;Type2&gt;(new Type2(<em>args</em>));

Type1( ) : var(unique_ptr&lt;Type1&gt;(new Type2(<em>args</em>))) {. . .}

However, if you use std::move it will not be an error.

<em> </em>

<em> </em>

<em> </em>

<em>Second bit of useful information: </em>

C++ tries to make the semantics of shared_ptr match the semantics of raw C pointers as closely as possible. It also tries to make the semantics of make_shared mimic the semantics of new as much as possible. As a result, if you code:

std::shared&lt;AbstractBase&gt; sp = std::make_shared&lt;AbstractBase&gt;(DerivedNotAbstract( ));

this mimicking new you will cause you to get an error since std::make_shared&lt;AbstractBase&gt; will be treated as an attempt to create a new AbstractBase object. The correct way to do this is to do:

std::shared&lt;AbstractBase&gt; sp = std::make_shared&lt;<strong>DerivedNotAbstract</strong>&gt;(DerivedNotAbstract());

For those of you that remember last semester when I talked about the desire to program to interfaces, the use of the concrete DerivedNotAbstract twice should be bothersome. This can be avoided by having a factory method, std::shared_ptr&lt;AbstractBase&gt; factory(…) {…}  and use it to do the initialization, e.g.,

std::shared&lt;AbstractBase&gt; sp = factory(“Derived”); but this is not necessary for this homework.
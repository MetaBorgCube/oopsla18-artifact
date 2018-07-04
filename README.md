# Artifact for OOPSLA'18 Submission 'Scopes as Types'

## Artifact Description

This is the artifact accompanying the OOPSLA'18 submission 'Scopes as
Types'. This artifact consists of the following:

- The paper draft in `scopes-as-types.pdf`, which is updated to match
  the artifact. Note that the paper submitted as part of the HotCRP
  submission, is the original draft, which may deviate in places from
  this artifact.

- An implementation of the Statix language, embedded in the Spoofax
  language workbench. A test suite of unit tests for Statix is
  included in the artifact.

- Language implementations for the three case studies in the
  paper. Each implementation includes a static semantics definition in
  Statix, some example programs, and a test suite to test the static
  semantics. The implementations provide source code editors with
  syntax highlighting and type checking. The result of type checking
  is simply yes or no at the moment, and no detailed error messages
  are shown in the editors.

## Getting Started

This section explains how to get the artifact running and what can be
inspected. The artifact consists of a Spoofax Eclipse workspace with
language implementation projects for the case studies presented in the
paper. The static semantics of these languages are defined in Statix,
the constraint language defined in the paper. First we explain how to
get Spoofax, and build the language projects. Then we explain the
structure of the language projects, and what can be inspected and
tested.

### Getting Spoofax and Building the Language Projects

The case studies are implemented in the Spoofax language workbench,
which supports Statix is one of its meta-languages.

- Download the appropriate Spoofax archive for your platform from
  https://buildfarm.metaborg.org/job/metaborg/job/spoofax-releng/job/master/662/artifact/dist/spoofax/eclipse/. The
  `spoofax-*-jre.*` archives include a JRE, and do not depend on a
  compatible local Java installation. It is recommended to use a
  version with an included JRE.

- Install and run Spoofax by unpacking the archive, and starting
  `eclipse` or `eclipse.exe`. When Spoofax is started, it will ask for
  a workspace; select the artifact root directory. Workspaces can be
  changed using the `File > Switch Workspace` menu, if necessary.
  
- After start-up, wait until the task `Building workspace...` is not
  shown in the bottom right status bar anymore.
  
- Build all language projects using the `Project > Build All`
  menu. After a successful language build, the console shows something
  like:
  
      Reloading language project eclipse:///lang.sysf

  Individual languages can be rebuilt by selecting the project in the
  `Package Explorer`, and selecting `Project > Build` from the menu.

### Inspecting the Language Projects

The workspace contains three language projects, for the three case
studies: the Simply Typed Lambda Calculus with Structural Records in
`lang.stlcrec`, System F in `lang.sysf`, and Featherweight Generic
Java in `lang.fgj`. Each project contains a Statix specification of
the static semantics, some example programs, and a test suite.

- The Statix specification of a language is found in the file
  `trans/statics.stx`. The file can be opened in an editor by
  double-clicking.

- Example programs can be found in the `example` folder of a language
  project. Double-clicking opens an editor and starts type checking
  for the file, using the language's Statix specification. If the file
  contains errors, a red marker appears at the top of the
  editor. Depending on the size of the program, type checking can take
  some time. Only after type checking is done will the error marker be
  updated. When done, the console shows a status line similar to:

      Solved 415 constraints (103 delays) with 0 failed and 0 remaining constraint(s).

  If the number of failed or remaining constraints is not zero, the
  program did not successfully type check.

- Every language project includes a test suite for the static
  semantics of the language, located in the `test` directory. Test
  files have a `.spt` extension, and can be opened and inspected in an
  editor by double-clicking. Failing tests are marked with a red
  marker. If a file contains many tests, it may take a while before
  the tests are finished and the success and failures are correctly
  marked. Use the console or the progress window to check if the tests
  are still running.

- All tests in a directory can be run by selecting the directory in the
  `Package Explorer`, and selecting the `Spoofax (meta) > Run all
  selected tests` menu. A test runner will appear listing all tests,
  showing progress and which tests success or failure. Running all
  tests (especially for FGJ) takes quite a while. For faster results,
  open individual files, or run fewer tests by selecting
  subdirectories of the `test` directory.

The `statix.tests` project contains unit tests for Statix itself. It
contains a `test` folder with tests similar to the tests in the
language projects.

## Claims Supported by the Artifact

This artifact supports the following claims from the paper:

> "We show that viewing scopes as types enables us to model the
> internal structure of types in a range of non-simple type systems
> (including structural records and generic classes) using the generic
> representation of scopes. Further, we show that relations between
> such types can be expressed in terms of generalized scope graph
> queries. We extend scope graphs with scoped relations and queries."

The specifications of all three languages model non-syntactic aspects
of types, such as record and class structure, or type variables and
lazy substitution, using the scope graph model.

> "We extend the scope graph model with scoped relations to model the
> association of types with declarations and explicit substitutions in
> the instantiation of parametrized types. We generalize name
> resolution in scope graph from resolution from references to
> declarations to general queries for scoped relations. This enables
> flexible definition of queries for reachable or visible declarations
> and other properties, such as the visible record fields in the
> definition of subtyping of structural record types."

This extended model and a resolution algorithm are implemented as part
of the Statix implementation. Many tests for different resolution
scenarios are included in the `statix.tests` project, in the
`test/scopegraphs` directory.

> "We introduce Statix, a declarative, language for specifying type
> systems. The language provides simple guarded rules for definition
> of user-defined constraints with unification and scope graph
> construction and resolution as built-in theories. We provide a
> declarative and an operational semantics of Statix."

The Statix language, including a type checker and a solver, are
implemented and included as a part of Spoofax. The tests in
`statix.tests` document and test the behavior of the solver for
different Statix programs.

> "We simplify the resolution calculus and algorithm of Néron et
> al. [2015a] and Van Antwerpen et al. [2016] by not including imports
> as a primitive. We demonstrate how imports (and other name- and
> type-dependent name resolution schemas) can be encoded using the
> scopes as types approach. We discuss how these patterns depend on
> resolution in incomplete scope graphs, and how the algorithm
> guarantees soundness of resolution in incomplete graphs. We further
> generalize resolution by namespace/query-specific parameterization
> with visibility policies instead of global policies."

The case study specifications implement binding patterns -- such as
class inheritance -- using the simplified model that is a part of
Statix. In earlier work, these patterns were implemented using the
import mechanism. Global resolution policies, for example how to
resolve methods or variables in FGJ, are provided in the Statix
language for convenience, but every query can redefine every parameter
of the resolution calculus.

Statix provides integrated support for guaranteeing that resolution in
incomplete graphs is safe, following the principles discussed in the
paper Section 5.2. Specific tests for this behavior can be found in
the `statix.tests` project, in the `test/scopegraphs/relations.spt`
folder; search for tests with _incomplete_ in the name.

> "We have evaluated the Statix language in three case studies: the
> simply-typed lambda calculus with records [Pierce 2002] (STLC-REC),
> System F [Girard 1972; Reynolds 1974], and Featherweight Generic
> Java [Igarashi et al. 2001]."

The artifact contains type checkers for the case study
languages. These type checkers are not (yet) meant to be efficient,
nor to provide good error messages. For now, the type checkers merely
check whether programs type check or not. The test suites give us
trust in the correctness of the given specifications.

The following claims from the paper are not supported by this
artifact:

> "We extend the visual notation of scope graph diagrams with scoped
> relations, which provides a useful language for explaining patterns
> of names and types in programming languages. We also extend the
> visual notation with unresolved (constraint) nodes for illustrating
> the resolution process."

The visual notation that the paper introduces and demonstrates is not
a part of Statix or of its output.

## Case study languages

We briefly summarize the case study languages that we have
implemented. Each of the specifications were developed with a focus on
object language feature coverage, correctness, and clarity of
specification.

### STLCrec

This case study implements an extension of the simply-typed
lambda-calculus with records. The language contains the following
constructs:

- numbers and addition (`1 + 2`);

- functions and identifiers (`fun (x : num) { x }`), as well as
  function application (`f 1` where `f` is a function-typed
  expression);

- type ascription expressions (`1 + 2 : num`);

- `let` expressions (e.g., `let x = 1 in x`);

- records (`{x = 1}`) and record types (`{x : num}`) with structural
  subtyping;
  
- record extension expressions (`{x = 1} with r` where `r` is a
  record-typed expression); and
  
- type let binders and type identifier references (`type r = {x : num}
  in fun(y : r) { y.x }`).

#### Syntax, Semantics, and Example Files

The syntax of the language is given in
`lang.stlcrec/syntax/STLCrec.sdf3`.

The Statix semantics is given in `lang.stlcrec/trans/statics.stx`.

For object language tests, consult the files in `lang.stlcrec/test/`.

#### About the Semantics

Section 3.1 of the paper contains example illustrations of scope
graphs for STLCrec programs. Here we highlight some of the core
features of the semantics and its use of scopes as types:

- Record field names and function parameter names reside in separate
  namespaces, `Fld` and `Var` respectively. Each namespace has its own
  name resolution policy (see their `name-resolution` signatures in
  `lang.stlcrec/trans/statics.stx`).

- There are two notions of type in the language:
  1. Type expressions (`TypeExp`): syntactic type representations in
     object language programs.
  2. Types (`Type`): semantic type representations used in the type
     system specification.

- `typeOfTypeExp` translates type expressions (`TypeExp`) into types
  (`Type`).

- Record types (`REC`) are given by a scope in the scope graph.

- Record extension is modeled as an `R`-labeled edge in the scope
  graph; see, e.g., `typeOfExp(s, FExtend(e, finits))` in
  `lang.stlcrec/trans/statics.stx`.
  
- Records with duplicate field names are disallowed via the
  `unique`ness check in `fieldInitsOK` in `lang.stlcrec/trans/statics.stx`.

- Record subtyping is modeled using two queries over the scope graph
  (see `subType(REC(s_sub), REC(s_sup))` in
  `lang.stlcrec/trans/statics.stx`):
  1. the query given by `allFields` finds all visible fields in the
     super type.
  2. the query given by `subFields` checks that there is exactly one
     visible field of a matching (sub-)type for every field in the
     super type.

### System F

This case study implements System F with type let binders. The
language contains the following constructs:

- functions, identifiers, function application, let expressions, type
  ascription expressions, and type let binders (using the same syntax
  as STLCrec);

- type binders (`Fun(T) { fun(x : T) { x } }`) and forall type
  quantifiers (`T => T -> T`);
  
#### Syntax, Semantics, and Example Files

The syntax of the language is given in
`lang.sysf/syntax/SystemF.sdf3`.

The Statix semantics is given in `lang.sysf/trans/statics.stx`.

For object language tests, consult the files in `lang.sysf/test/`.

#### About the Semantics

We highlight some of the core features of our semantics for System F
and its use of scopes as types:

- Type parameter names and function parameter names reside in separate
  namespaces.

- Forall-types (`ALL`) are modeled as a scope with two declarations in
  it: one declaration that records the formal parameter; and one
  declaration that is associated with the type of the body of the
  forall-type. In this sense, the `ALL` type is reminiscent of a
  two-field record.
  
- Type application uses the `instWith` relation to add a substitution
  to the scope of an `ALL` type, and projects the body from the
  resulting record (see `typeOfExp(s, TApp(e, t)) = T` in
  `lang.sysf/trans/statics.stx`). Projections are evaluated lazily:
  the `PROJ` type represents a lazily postponed projection.
  
- When we match on a type we first apply `strict` to it, which
  normalizes `PROJ` types by stricting the postponed projection. The
  type resuling from applying `strict` is in WHNF: the substitution is
  pushed inwards in a lazy fashion. The precise definitions of `strict`
  and type `norm`alization are given in
  `lang.sysf/trans/statics.stx`. Laziness is not built into Statix,
  so the lazy substitution strategy is encoded rather explicitly in
  the aforementioned definition of `norm`. `strict` and `norm` are
  mostly generically defined: the same notions are used to implement
  generics in FGJ, as described in the paper Section 3.3.

- When we compare two types (e.g., in `typeOfExp(s, App(e1, e2))` in
  `lang.sysf/trans/statics.stx`) we use `typeEq` which implements the
  following scheme for comparing types:
  
  1. If either of the types we are comparing is a `PROJ`ection, apply
     strictness.
  
  2. Numbers and function types are compared in the obvious
     compositional way.
     
  3. `ALL`-types are compared by inventing a fresh place holder value
     `PL` and substituting this in each of the quantified types for
     the two `ALL` types that we are comparing. We then compare the
     quantified types by projecting the bodies from the scopes with
     these substitutions.

### Featherweight Generic Java (FGJ)

This case study language implements FGJ. The language contains the
same constructs as in the original paper [Featherweight Java: a
minimal core calculus for Java and
GJ](https://doi.org/10.1145/503502.503505) by Igarashi, Pierce, and
Wadler. Our FGJ language has a few minor differences in syntax and
semantics from the original FGJ language:

- In our variant of FGJ, it is not mandatory that each class has a
  constructor parameter for each field in the class.
  
- In our variant of FGJ, fields can be initialized with arbitrary
  expressions, whereas original FGJ only allows variable references as
  the right-hand side of field initialization expressions.

- Our variant of FGJ does not check that every field is explicitly
  initialized.

#### Syntax, Semantics, and Example Files

The syntax of the language is given in
`lang.fgj/syntax/FGJ.sdf3`.

The Statix semantics is given in `lang.fgj/trans/statics.stx`.

For object language tests, consult the files in `lang.fgj/test/`.

#### About the Semantics

Section 3.2 and 3.3 of the paper contains example illustrations of
scope graphs for STLCrec programs. Here we highlight some of the core
features of the semantics and its use of scopes as types:

- Inheritance is represented using a dedicated "super" edge in the
  scope graph.

- Subtyping is defined in terms of a query over the scope graph: the
  `extendsQ` constraint represents a query which checks that we can
  traverse a sequence of "super" edges in the graph to connect a
  sub-class with its super-class.
  
- Generic type parameter substitution and class type comparison is
  implemented using similar machinery as summarized for System F
  above.

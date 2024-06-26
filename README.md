# treeiterator

TreeIterator is a pattern for declarative (fluent) iterators that allow commands to be applied to each tree node. A command consists in its simplest form of a condition and an operation. The pattern is useful for the manipulation of tree objects like XML or JSON and information gathering. The tree is iterated only once. The current implementation is in Java but the pattern is not language specific.
Details regarding the Java implementation are at the end of this document.

### Structure

    TreeIterator.topDown(tree)
        .condition().operation()
        .condition().and().condition().operation()
        ...
        .execute()
    
### Conditions

    .when()
    .whenNot()
    .always()
    
Specific conditions can be also implemented to improve readibility. Some examples:

 - exact match: whenId(), whenPath()
 - pattern match: whenIdMatches(), whenPathMatches()
 - node type: whenLeaf(), whenNotLeaf(), whenRoot()
 - occurrence index: whenFirst(), whenLast(), whenNth()
 - level: whenOnLevel()
 
### Operations

    .then(consumer()) // executes consumer 
    .remove() // removes node 
    .replace(supplier()) // replaces the node
    .skip() // skips the whole subtree starting with the node the condition matches (only works in topDown() mode)
    .stop() // stop the iteration
    
The operation names are based on the [Java Functional Interfaces](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)

### Arguments

In the simplest form conditions and operations will have the tree node as their argument.
Depending on the need, further node information (which is not provided by the node itself) can be added.
A list of possible arguments:

	node, id, path, level, parent, ancestors, descendants

Arguments can be encapsulated in a **IterationStep** object.

### Traversal Direction

    .topDown()
    .bottomUp()
    
### Design Goals

**Simple structure**: follow a simple structure for the fluent language and allow flexibility through combinations.

**Completeness**: provide all possibly necessary conditions and operations to maximize the usefulness of the iterator.

### Credits

The ideas for declarative tree iterators were brainstormed by Eduard Beutel and Grebiel Ifill.

### Java Implementation

JDK21 [![codecov](https://codecov.io/gh/eduardbeutel/treeiterator/graph/badge.svg?token=QDND4FUF1R)](https://codecov.io/gh/eduardbeutel/treeiterator)

Code that can be reused to implement this pattern for any tree data structure is in the common package.

Supported trees:

- org.w3c.dom.Element and .Document by ElementTreeIterator

Conditions:

- always()
- when()
- whenNot()
- whenId()
- whenPath()
- whenIdMatches()
- whenPathMatches()
- whenRoot()
- whenLeaf()
- whenNotLeaf()

Operations:

- then()
- skip()
- remove()
- replace()
- set()
- collect(), collectById(), collectByPath()

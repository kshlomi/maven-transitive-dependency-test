This project reproduces a case where test scope transitive dependency versions override  compile scope transitive dependencies.

The defined dependencies:
```
bar
    -> foo (compile)
        -> transitive:1.0 (compile)
    -> some-test-lib (test)
        -> transitive:1.1 (compile) 
```

Reproducing the issue:
```
 ❯ mvn clean install -am -pl ./bar
 ❯ mvn dependency:tree -pl bar
[INFO] Scanning for projects...
[INFO]
[INFO] --------------------------< org.example:bar >---------------------------
[INFO] Building bar 1.0
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ bar ---
[INFO] org.example:bar:jar:1.0
[INFO] +- org.example:some-test-lib:jar:1.0:test
[INFO] |  \- org.example:transitive:jar:1.1:compile
[INFO] \- org.example:foo:jar:1.0:compile
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.897 s
[INFO] Finished at: 2022-07-11T16:19:38+03:00
[INFO] ------------------------------------------------------------------------
```

Obviously this can be worked around by reversing dependency order in `bar` or by dependency exclusion by we're after a more generic solution.
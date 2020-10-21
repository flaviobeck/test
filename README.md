run:

mvn clean test org.pitest:pitest-maven:mutationCoverage
--> regular pitest execution:  will create HTML output with line coverage and mutation coverage

mvn test -DoutputFormats=XML -DfullMutationMatrix=true org.pitest:pitest-maven:mutationCoverage
--> full matrix execution: will not clear previous output, but add mutations.xml matrix output

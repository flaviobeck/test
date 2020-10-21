PROCEDIMENTO PARA OS EXPERIMENTOS:

1 - Executar em um projeto Java/Maven
2 - Verificar se os testes Unitarios do projeto atendem ao criterio de cobertura minima e nada mais alem do minimo
3 - Verificar se o projeto possui pelo menos um mutante sobrevivente (stubborn)
4 - Criar 10 conjuntos de testes com inputs randomicos (mesmos testes, porem com valores diferentes de entrada )
5 - Verificar se os mutantes sobreviventes para cada conjunto sao diferentes


-----


run:

mvn clean test org.pitest:pitest-maven:mutationCoverage
--> regular pitest execution:  will create HTML output with line coverage and mutation coverage

mvn test -DoutputFormats=XML -DfullMutationMatrix=true org.pitest:pitest-maven:mutationCoverage
--> full matrix execution: will not clear previous output, but add mutations.xml matrix output

Exported Jenkins Pipeline
// Powered by Infostretch 

timestamps {

node () {

	stage ('test3 - Checkout') {
 	 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/flaviobeck/test.git']]]) 
	}
	stage ('test3 - Build') {
 	
withEnv(["JAVA_HOME=${ tool '"+JDK+"' }", "PATH=${env.JAVA_HOME}/bin"]) { 

// Unable to convert a build step referring to "hudson.plugins.sonar.SonarBuildWrapper". Please verify and convert manually if required.		// Maven build step
	withMaven(jdk: 'JDK 11', maven: 'Maven 3.6.1') { 
 			if(isUnix()) {
 				sh "mvn clean test -DoutputFormats=XML,HTML org.pitest:pitest-maven:mutationCoverage sonar:sonar " 
			} else { 
 				bat "mvn clean test -DoutputFormats=XML,HTML org.pitest:pitest-maven:mutationCoverage sonar:sonar " 
			} 
 		}
// Unable to convert a build step referring to "hudson.plugins.sonar.SonarRunnerBuilder". Please verify and convert manually if required.		// Maven build step
	withMaven(jdk: 'JDK 11', maven: 'Maven 3.6.1') { 
 			if(isUnix()) {
 				sh "mvn test -DoutputFormats=XML -DfullMutationMatrix=true org.pitest:pitest-maven:mutationCoverage " 
			} else { 
 				bat "mvn test -DoutputFormats=XML -DfullMutationMatrix=true org.pitest:pitest-maven:mutationCoverage " 
			} 
 		} 
	}
}
}
}

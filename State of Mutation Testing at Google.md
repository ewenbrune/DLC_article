# State of Mutation Testing at Google  

## Mutation Testing  
Mutation testing assesses test suite efficacy by inserting small faults into programs and measuring the ability of the test suite to detect them.   

### Traditional approach  
The traditional approach consist of creating ```mutants``` wich are copies of the original program under test but with only one difference. The number of created mutants is arbitrary and can be choosen by stopping criterion.   

#### Example  
Original Code :  
``` java
int RunMe(int a, int b) {
	if (a == b || b == 1) {
		return ;
	}
	return 2;
}
```

Mutant : 
``` java
int RunMe(int a, int b) {
	if (a != b || b == 1) { // Mutation here (a != b)
		return ;
	}
	return 2;
}
```

The test suite is then ran, if at least one test case fail, the mutant is considered ```dead``` otherwise it is reffered as ```alive```. 
 
To measure the efficiency of the test suite a metric named ```mutation score``` is used. Mutation score is the ratio of killed mutants to the total number of mutants created. 

Mutations are done using so called ```mutators```, these are a set of predifined transformation to apply on the original code to create mutants. The naïve strategy is to use random mutators on random lines in the source code to generate mutants.  

### Limitations of mutation testing  
Traditional mutation testing is extremly efficient as a stopping criterion but it is extremly computationaly expensive. Moreover some mutation do not invalidate the program semantic wich is leading to flase positive. Filtering the result is tedious when the number of mutants is high.  


### Google's approach
To address  mutation testing's limitation Google created it's own tool enabling mutation testing to be used at a large scale (~6,000 engineers with 40000 dailly commits even tho only 30% off diffs are processed). 

The tool used is named Critique. It is an automated tool integrated in the development workflow and is surfacing its result during code reviews.   


#### Mutant finding shown in the Critique - Google code review tool
![[Critique_Google.png]]   

```Please fix``` and ```Not useful``` are used to determinate if the result surfaced is useful or not. The ```Not useful``` link will send a feedback to the creator of the tool in order to futher enhanced its quality.  

### How it works  
To minimize the number of mutant, Google is using heuristics to filter un-interesting mutant. The systems is using the AST of the code. The heuristics is then trying to find ```arid``` node in the AST (e.g non-intersiting node) multiple rules which are language specific.  

#### Arid Nodes  

Heuristics are used todsf 
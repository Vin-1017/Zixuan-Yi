# Recursion

Solving a problem using recursion depends on solving smaller occurence of the same problem.

##### Recursive M&M bowl

```pseudocode
doubleMnMs(bowl):
	if bowl is empty:
		hand bowl back to previous person.
	else:
		take 1 M&M out.
		hand bowl to the next person; tell them to doubleMnMs.
		when they hand the bowl back, put 2 M&Ms in.
		hand bowl back to previous person.
```

##### Cases

-base case
-recursive case

##### Example

```c++
int factorial(int n){
  if (n == 0 || n == 1){
    //base case
    return 1;
  } else{
    //recursive case
    return n * factorial(n - 1);
  }
}
```

```c++
bool isPalindrome(string s){
  if (s.length()<=1){
    return true;
  } else{
    char first = s[0];
  	char last = s[s.length() - 1];
    string middle = s.substr(1, s.length() - 2);
    return (first == last && isPalindrome(middle));
  }
}
```

```c++
class HanoiGui{
  void initialize(int numDisc);
  void moveDisc(int startDeg, int endDeg);
  void thirdPegNumber(int peg1, int peg2);
};
void moveDiscs(int numDiscs, int startPeg, int endPeg){
  int thirdPeg = HanoiGui::thirdPegNumber(startPeg, endPeg);
  if (numDisc == 1){
    return;
  } else{
    moveDiscs(numDisc - 1, startPeg, thirdPeg);
    HanoiGui::moveDisc(startPeg, endPeg);
    moveDiscs(numDisc - 1, startPeg, endPeg);
  }
}
```

```c++
/*
 *---------------------------
 *---------         ---------
 *---   ---         ---   ---
 *- -   - -         - -   - -
 */
void cantorSet(GWindow& window, int x, int y, int length, int levels){
  if (levels == 0){
    //base case - do nothing
  } else{
      //recusive case
      window.drawLine(x, y, x + length, y); // the place wnere put this line effects the sequence of drawing
			cantorSet(window, x, y + 20, length/3, levels - 1);
    	cantorSet(window, x + 2*length/3, y + 20,length/3, levels - 1);
  }
}
```

```c++
int evaluateHelper(string exp, int index){
  char ch = exp[index];
  if (ch == '('){
    index ++; //move past ( character
    int left = evaluateHelper(exp, index);
    char opr = exp[index++];
    index ++; //move past ) character
    if (opr == '+'){
      return left + right;
    } else {
      return left * right;
    }
  } else{
    return charToInteger(ch);
  }
}
int evaluate(string s){
  return evaluateHelper(exp, 0);
}
```

#### Exhaustive Search and Backtracking

##### Exhausive Search

Exploring every possible combination from a set of choices or values.

```pseudocode
Explore(decisions):
	-if there are no more decisions to make: Stop.
	-else, handle one decision ourselves, and the rest by recursion.
	 for each available choice C for this decision:
	 	Choose C.
	 	Explore the remaining decisions.
```

```c++
void printAllBinaryHelper(int digits, string output){
  if (digits == 0){
    //base case: print the string we have built;
    cout << output <<endl;
  } else{
    printAllBinaryHelper(digits - 1, output + "0");
    printAllBinaryHelper(digits - 1, output + "1");
  }
}
void printAllBinary(int digits){
  printAllBinaryHelper(digits, "");
}
```

##### Backtracting

Finding solution(s) by trying partial solutions and then anandoning them if they are not suitable.

```pseudocode
Explore(decisions):
	-if there are no more decisions to make: Stop.
	-else, handle one decision ourselves, and the rest by recursion.
	 for each available choice C for this decision:
	 	Choose C.
	 	Explore the remaining decisions.
	 	UNCHOOSE C.
```

```c++
void diceSumHelper(int dice, int desiredSum, vector<int>& chosen){
  if (dice == 0){
    //Can't do it like if(dice == 0 && desiredSum == 0)
    if(desiredSum == 0){
      //base case
      cout << "(" ;
      for (auto i : chosen) cout << i <<' ';
      cout<< ")" << endl;
    }
    //base case
    cout << "(" ;
    for (auto i : chosen) cout << i <<' ';
    cout<< ")" << endl;
  } else {
    for (int i = 1; i <= 6; i++){
      //choose
      chosen.push_back(i);
      //explore
      diceSumHelper(dice - 1,desired - i, chosen);
      //un-choose
      chosen.pop_back();
    }
  }
}
void diceSum(int dice, int desiredSum){
	vector<int> v;
  diceSumHelper(dice, desiredSum, v);
}
```

Reference: https://www.youtube.com/playlist?list=PLfmlprXA8kc9AAAC3hcg-35MAvVxT2F3
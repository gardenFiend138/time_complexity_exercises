# Time Complexity Practice

### Let's start with some array manipulation to get us warmed up. Find the time complexity of each of the following functions (in order -- they depend on each other). You may assume that all arrays are arrays of integers, for convenience. When you evaluate time complexity, remember to do each of the following:

1. Specify which aspect (or aspects) of the input the time complexity depends on. E.g., if a function is O(n), what is n?
2. Explain thoroughly and clearly why the time complexity is what it is.
3. Find the worst cases.
4. Discuss space complexity too: this is usually deemphasized over time complexity, but some interviewers will ask about it.

* Notes are taken to show my thinking--some may start out stating one thing, but will change if I come to another conclusion.
--- 

1. 
```ruby 
def add(a, b)
  if a > b
    return a + b
  end

  a - b
end
```
* Since the method simply compares the numbers and performs an operation, the time complexity for this adding method is constant (O(1)). The best and worst case scenarios are the same for this method since it will be the same regardless of the input. Time complexity for this method is also O(1), since no additional variables are assigned in evaluating the method.

2. 
``` ruby 
def print_arr_1(arr)
  arr.each do |idx|
    puts el
  end
end
```
* The time complexity for this method also depends on the input, but it is dependent on the size of the input. The time complexity of this method is O(n), where `n` is the size of the array. This method won't print anything to the screen, since Ruby's .each creates a local variable, named here `idx`, which is never used in the block. This method would throw an error, since `el` is not defined. Since this method will never actually run, it will be O(1), since it will always throw an error on the first iteration. This means that, since it is constant, it is not dependent on the size of the input. O(1) -- Final answer. The space complexity for this would also be O(1), since only one variable is created, `idx`. 

3. 
```ruby 
def print_arr_2(arr)
  arr.each_with_index do |el, idx|
    break if idx == arr.length/2 - 1
    puts el
  end
end
```
* Even though this method will only iterate over and print only half the array, it still has O(n) time complexity, since arr.length/2 - 1 evaluates to n/2 - 1, with `n` being the size of the array. Since the integers are constant, we ignore those with asymptotic notation. The `puts` statement also runs in constant time, so this we can ignore, since this would give us n/2 + 1 for our overall run time. The space complexity of this method is O(1), since only two variables are created, `el` and `idx`, and are reassigned each iteration. 

4. 
```ruby 
def print_arr_3(arr)
  arr.each do |el|
    break if el == arr.length/2 - 1
    puts el
  end
end
```

* The time complexity for this method is dependent on the size of the array, as well as the integers themselves. In the best case scenario, if the first element in the array is equivalent to n/2 - 1, the time complexity would be O(1), but at it's worst, would be O(n), since the element that would break the method could either be the last element in the array, or could not be present at all. Since we are only concerning ourselves with the worst case performance, it is O(n). The space complexity for this is also constant since only one local variable, `el`, is assigned for this method. Although, `.each` still has to create an indexing variable under the hood, so there are actually 2 variables created, but since the number is still constant, it remains O(1). 

5. 
```ruby 
def print_arr_4(arr)
  arr.each do |el|
    break if el == arr.length/2 - 1
    puts el
  end

  arr.each_with_index do |el, idx|
    puts el if idx % 3 == 0
  end

  puts arr.last
end
```

* This method calls `.each` twice, so my initial thought it O(n), since the iterations are not nested within each other. The first call will at best be O(1), since the first element in the array may cause that block to break out early, and at it's worst would be O(n), since the element that is equivalen to n/2 - 1 may not be present in the array. For the second loop, the run time is O(n), since it will iterate over the whole array, but only print out elements if their index is evenly divisible by three. The final call in the method is to print the last element of the array, and since indexing into arrays and printing is constant, the final line is O(1) as well.  The overall time complexity for this method is: 2n + 1, which reduces to O(n) once we ignore the constants. The space complexity for this method is constant as well, since there are always 4 variables created and reassigned (including the indexing variable created under the hood for `.each`). 

6. 
```ruby 
def search(arr, target)
  arr.each_with_index do |el, idx|
    return idx if el == target
  end
end
``` 

* The time complexity of this method is dependent on the size of the array. In the best case scanario, the run time is O(1), since the first element in the array may be the target, which would cause the return statement to evaluate as true. At worst, the element could not exist in the array, or could be the last element, which would lead to O(n), since the entire array would have to be iterated over regardless. The space complexity of this is O(1), since two local variables are created, `el` and `idx`. 

7. 
```ruby 
def searchity_search(arr, target)
  results = []
  arr.each do |el|
    results << search(arr, target + el)
  end

  results  
end
```

* Since this method calls the previous method within its loop, it would be O(n^2). The `.each` call in this method would be O(n), since the entire array is iterated over, but then within each iteration, a call to `search` is made, which is also O(n), and leads to the overall time complexity of O(n^2), where `n` is dependent on the size of the array. The space complexity for this method is O(n), since there are 3 local variables created (2 in the `search` call, 1 in this method call), as well as an array of the results, which makes `n` dependent on the elements in the array.

8. 
```ruby 
def searchity_search_2(arr, target)
  results = []
  arr.each do |el|
    results << search(arr, el)
  end

  results  
end
```

* This method will call the search method from number 6, so we will have to take the previous method's time and space complexity into account. This function has a time complexity of O(n) for iterating over the array, with `n` being the size of the array. Within the loop, the previous  `search` function is called, which had a worst case run time of O(n). Since this method is called within the loop, the overall worst case time complexity is O(n^2). This will always be the case for this method though, since each time `search` is called, it is using the current `el` as the target, which will always be found in the array. Since there is a pattern to how many times the inner `search` function will be called, I think it will actually be better then O(n^2). For an array with 5 elements, `searchity_search_2` will still loop through the whole array, running in O(n) time, but the first time `search` is called, it will return the 0th index, the second time it will return the 1st index, the third time it will return the 2nd index, but in the end, it will still need to loop through the entire array in the inner `search` call, so I'm sticking with O(n^2), even though I thought it may have been O(n log n).

---

### Iterativeness 

1. 
```javascript 
let iterative_1 = (n, m) => {
  let notes = ["do", "rei", "mi", "fa", "so", "la", "ti", "do"];

  for (var i = 0; i < n; i++) {
    for (var j = 0; j < m; j++) {
      let position = (i+j) % 8;
      console.log(notes[position]);
    }
  }
}
```

* The time complexity of this funciton is dependent on the arguments, since the number of iterations is dependent on the arguments. This leads to a time complexity of O(n * m), since for each iteration `n`, `m` iterations are made in the inner loop. The print statement runs in constant time, so we can ignore that. For space complexity, we assign 4 local variables, which is a fixed amount, and means that the space complexity is constant. 

2. 
```javascript 
let iterative_2 = (n) => {
  let notes = ["do", "rei", "mi", "fa", "so", "la", "ti", "do"];

  for (var i = 0; i < n; i++) {
    for (var j = i; j >= 0; j--) {
      let position = (i+j) % 8;
      console.log(notes[position]);
    }
  }
}
```

* This function is similar to the previous one, execpt for the fact that instead of incrementing `j`, we decrement it, and it only takes one argument. If we say that `n = 5`, then the outer loop would run 5, or `n` times, and the inner loop would run 0 times, then once, then twice, then three times, etc, until it has run 5 times. Since the inner loop will still eventually be O(n), the overall time complexity of this function is O(n^2). The space complexity is the same as the above function, O(1), for the same reasons as stated above. 

3. 
```javascript
let iterative_3 = (n, m) => {
  let notes = ["do", "rei", "mi", "fa", "so", "la", "ti", "do"];

  let bigger = n > m ? n : m;
  let smaller = n <= m ? n : m;

  for (var i = 0; i < smaller; i++) {
    for (var j = i; j < bigger; j++) {
      let position = (i+j) % 8;
      console.log(notes[position]);
    }
  }
}
```
 * This function is still O(n * m), for the same reasons outline in problem 1. The space complexity remains constant as well, because even though we are assigning two new variables, `bigger` and `smaller`, the number of variables we assign remains constant, as does the memory used. 
 
 ---
 
 ### Radical Recursion
 
* Recursive functions are among the toughest to evaluate for time complexity. Remember FFS:

  * __F__ind the time complexity of one call, ignoring the recursive calls.
  * __F__ind the number of times the function is called, and, if necessary, the input sizes on all of those calls.
  * __S__um everything together.
 
 1. 
 ```ruby 
 def rec_mystery(n)
  return n if n < 5

  rec_mystery(n - 5)
end
 ```
 
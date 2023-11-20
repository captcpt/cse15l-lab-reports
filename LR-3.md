## Part I
### Buggy Code
```{java}
public class ArrayExamples {
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }
}
```

### JUnit Tests with Bug
#### Failure Case
```{java}
public class ArrayExampleTests {
  @Test
  public void testReversed() {
    int[] input = {1, 2, 3, 4, 5};
    int[] result = ArrayExamples.reversed(input);
    assertArrayEquals(new int[]{5, 4, 3, 2, 1}, result);
    assertArrayEquals(new int[]{1, 2, 3, 4, 5}, input); // Ensure input array is not modified
  }
}
```
`arrays first differed at element [0]; expected:<5> but was:<0>`

#### Successful Case
```{java}
public class ArrayExampleTests {
  @Test
  public void testReversedWithBug() {
    int[] input = {1, 2, 3, 4, 5};
    int[] result = ArrayExamples.reversed(input);

    // This test is checking for the bug, so it expects the original array to be modified
    assertArrayEquals(result, input);
  }
}
```
`OK (1 test)`

#### Symptom
<img width="382" alt="image" src="https://github.com/captcpt/cse15l-lab-reports/assets/84103589/3d203e74-64c0-4690-b9fd-79889e52000c">

### Corrected Code
```{java}
public class ArrayExamples {
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
  }
}
```

To fix the bug, I changed `arr[i] = newArray[arr.length - i - 1];` to `newArray[i] = arr[arr.length - i - 1];` and changed the return statement from `return arr;` to `return newArray;`.
This fixed the bug because the intended purpose of the method was to return a new array that was in reverse order of the input array `arr`, however the original (buggy) code was altering the input array `arr` instead of `newArray`, which caused it to provide the wrong order of values.

## Command Line Research (grep)
### `-w`
`grep -w` matches only on whole words for provided pattern (for example "gene" will only return "gene" and not something like "genetic")

### Example 1:
<img width="324" alt="image" src="https://github.com/captcpt/cse15l-lab-reports/assets/84103589/dc2618bd-8387-4804-93fb-b93405fdde21">

### Example 2:
<img width="310" alt="image" src="https://github.com/captcpt/cse15l-lab-reports/assets/84103589/6be3e898-c0ab-4dba-bc4a-49f5f0840cb7">

### `-c`
`grep -c` counts the number of matches for the specified pattern.

### Example 1:
<img width="296" alt="image" src="https://github.com/captcpt/cse15l-lab-reports/assets/84103589/e4611d07-8439-402c-8187-4a096f21614c">

### Example 2:
<img width="292" alt="image" src="https://github.com/captcpt/cse15l-lab-reports/assets/84103589/fd0d758c-b664-4c3c-98c1-fde3215c77ad">

### `-i`
`grep -i` makes the search case-insensitive; AKA will match on any case with same characters regardless if capitalized or not.

### Example 1:
<img width="294" alt="image" src="https://github.com/captcpt/cse15l-lab-reports/assets/84103589/e2d08eaf-15a8-406d-97ca-22f76b452e17">

### Example 2:
<img width="314" alt="image" src="https://github.com/captcpt/cse15l-lab-reports/assets/84103589/ed7181c0-759f-4b16-a48c-92b49cf9d358">

### `-n`
`grep -n` prints line numbers for matches as well

### Example 1:
<img width="318" alt="image" src="https://github.com/captcpt/cse15l-lab-reports/assets/84103589/becd5fe6-42b5-4699-b1f8-4aca4c0f75c3">

### Example 2:
<img width="314" alt="image" src="https://github.com/captcpt/cse15l-lab-reports/assets/84103589/644c0daf-6ef4-4545-a952-c048bdfef8d4">

Commands found from: https://www.geeksforgeeks.org/grep-command-in-unixlinux/

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

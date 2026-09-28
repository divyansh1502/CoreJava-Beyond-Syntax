# 💻 Java Coding Questions — Interview Questions & Answers

> **A focused collection of Java coding interview questions with concise answers, covering logic, arrays, strings, collections, recursion, searching, sorting, and common interview patterns.**

---

# 📑 Table of Contents

- [1. Numbers](#1-numbers)
- [2. Strings](#2-strings)
- [3. Arrays](#3-arrays)
- [4. Searching](#4-searching)
- [5. Sorting](#5-sorting)
- [6. Collections](#6-collections)
- [7. Recursion](#7-recursion)
- [8. Patterns and Logic](#8-patterns-and-logic)
- [9. Common Interview Coding Questions](#9-common-interview-coding-questions)
- [10. Rapid-Fire Coding Revision](#10-rapid-fire-coding-revision)

---

# 1. Numbers

## 1. Write a program to check whether a number is even or odd.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int n = 10;

        if (n % 2 == 0) {
            System.out.println("Even");
        } else {
            System.out.println("Odd");
        }
    }
}
```

---

## 2. Write a program to check whether a number is prime.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int n = 29;
        boolean prime = true;

        if (n < 2) {
            prime = false;
        }

        for (int i = 2; i * i <= n; i++) {

            if (n % i == 0) {
                prime = false;
                break;
            }
        }

        System.out.println(prime ? "Prime" : "Not Prime");
    }
}
```

---

## 3. Why do we check only up to `sqrt(n)` for prime checking?

### Answer

If `n` has a factor greater than `sqrt(n)`, it must have a corresponding factor smaller than `sqrt(n)`.

Therefore, checking:

```java
for (int i = 2; i * i <= n; i++)
```

is sufficient.

Time complexity:

```text
O(sqrt(n))
```

---

## 4. Write a program to reverse a number.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int n = 12345;
        int reverse = 0;

        while (n != 0) {

            int digit = n % 10;
            reverse = reverse * 10 + digit;
            n /= 10;
        }

        System.out.println(reverse);
    }
}
```

---

## 5. Write a program to check whether a number is a palindrome.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int n = 121;
        int original = n;
        int reverse = 0;

        while (n != 0) {

            int digit = n % 10;
            reverse = reverse * 10 + digit;
            n /= 10;
        }

        System.out.println(original == reverse);
    }
}
```

---

## 6. Write a program to count digits in a number.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int n = 12345;
        int count = 0;

        while (n != 0) {
            count++;
            n /= 10;
        }

        System.out.println(count);
    }
}
```

---

## 7. Write a program to find the sum of digits.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int n = 12345;
        int sum = 0;

        while (n != 0) {

            sum += n % 10;
            n /= 10;
        }

        System.out.println(sum);
    }
}
```

---

## 8. Write a program to find factorial of a number.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int n = 5;
        long factorial = 1;

        for (int i = 1; i <= n; i++) {
            factorial *= i;
        }

        System.out.println(factorial);
    }
}
```

---

## 9. Write a program to print Fibonacci numbers.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int n = 10;

        int a = 0;
        int b = 1;

        for (int i = 0; i < n; i++) {

            System.out.print(a + " ");

            int next = a + b;
            a = b;
            b = next;
        }
    }
}
```

Output:

```text
0 1 1 2 3 5 8 13 21 34
```

---

## 10. Write a program to find GCD of two numbers.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int a = 36;
        int b = 48;

        while (b != 0) {

            int temp = b;
            b = a % b;
            a = temp;
        }

        System.out.println(a);
    }
}
```

The algorithm is based on the Euclidean algorithm.

---

## 11. Write a program to find LCM of two numbers.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int a = 12;
        int b = 18;

        int x = a;
        int y = b;

        while (y != 0) {

            int temp = y;
            y = x % y;
            x = temp;
        }

        int gcd = x;
        int lcm = (a / gcd) * b;

        System.out.println(lcm);
    }
}
```

---

# 2. Strings

## 12. Reverse a String.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        String s = "Java";
        String reverse = "";

        for (int i = s.length() - 1; i >= 0; i--) {
            reverse += s.charAt(i);
        }

        System.out.println(reverse);
    }
}
```

For repeated concatenation, `StringBuilder` is generally preferable.

---

## 13. Reverse a String using StringBuilder.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        String s = "Java";

        String reverse = new StringBuilder(s)
                .reverse()
                .toString();

        System.out.println(reverse);
    }
}
```

---

## 14. Check whether a String is palindrome.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        String s = "madam";

        String reverse = new StringBuilder(s)
                .reverse()
                .toString();

        System.out.println(s.equals(reverse));
    }
}
```

---

## 15. Count vowels in a String.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        String s = "Java Programming";
        int count = 0;

        for (int i = 0; i < s.length(); i++) {

            char ch = Character.toLowerCase(s.charAt(i));

            if (ch == 'a' ||
                ch == 'e' ||
                ch == 'i' ||
                ch == 'o' ||
                ch == 'u') {

                count++;
            }
        }

        System.out.println(count);
    }
}
```

---

## 16. Count frequency of a character.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        String s = "banana";
        char target = 'a';

        int count = 0;

        for (char ch : s.toCharArray()) {

            if (ch == target) {
                count++;
            }
        }

        System.out.println(count);
    }
}
```

---

## 17. Find the first non-repeating character.

### Answer

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class Test {

    public static void main(String[] args) {

        String s = "swiss";

        Map<Character, Integer> map = new LinkedHashMap<>();

        for (char ch : s.toCharArray()) {
            map.put(ch, map.getOrDefault(ch, 0) + 1);
        }

        for (Map.Entry<Character, Integer> entry : map.entrySet()) {

            if (entry.getValue() == 1) {
                System.out.println(entry.getKey());
                break;
            }
        }
    }
}
```

`LinkedHashMap` preserves insertion order.

---

## 18. Check whether two Strings are anagrams.

### Answer

```java
import java.util.Arrays;

public class Test {

    public static void main(String[] args) {

        String a = "listen";
        String b = "silent";

        char[] x = a.toCharArray();
        char[] y = b.toCharArray();

        Arrays.sort(x);
        Arrays.sort(y);

        System.out.println(Arrays.equals(x, y));
    }
}
```

---

## 19. Remove duplicate characters from a String.

### Answer

```java
import java.util.LinkedHashSet;

public class Test {

    public static void main(String[] args) {

        String s = "programming";

        LinkedHashSet<Character> set = new LinkedHashSet<>();

        for (char ch : s.toCharArray()) {
            set.add(ch);
        }

        StringBuilder result = new StringBuilder();

        for (char ch : set) {
            result.append(ch);
        }

        System.out.println(result);
    }
}
```

---

## 20. Count words in a String.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        String s = "Java is powerful";

        String[] words = s.trim().split("\\s+");

        System.out.println(words.length);
    }
}
```

---

# 3. Arrays

## 21. Find the largest element in an array.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {10, 5, 30, 20};

        int max = arr[0];

        for (int i = 1; i < arr.length; i++) {

            if (arr[i] > max) {
                max = arr[i];
            }
        }

        System.out.println(max);
    }
}
```

---

## 22. Find the smallest element in an array.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {10, 5, 30, 20};

        int min = arr[0];

        for (int i = 1; i < arr.length; i++) {

            if (arr[i] < min) {
                min = arr[i];
            }
        }

        System.out.println(min);
    }
}
```

---

## 23. Find the sum of array elements.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {1, 2, 3, 4, 5};

        int sum = 0;

        for (int value : arr) {
            sum += value;
        }

        System.out.println(sum);
    }
}
```

---

## 24. Find the second largest element.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {10, 5, 30, 20};

        int largest = Integer.MIN_VALUE;
        int second = Integer.MIN_VALUE;

        for (int value : arr) {

            if (value > largest) {

                second = largest;
                largest = value;

            } else if (value > second && value != largest) {

                second = value;
            }
        }

        System.out.println(second);
    }
}
```

---

## 25. Reverse an array.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {1, 2, 3, 4, 5};

        int left = 0;
        int right = arr.length - 1;

        while (left < right) {

            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;

            left++;
            right--;
        }

        for (int value : arr) {
            System.out.print(value + " ");
        }
    }
}
```

This uses the two-pointer technique.

---

## 26. Find the missing number from `1` to `n`.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {1, 2, 4, 5};

        int n = arr.length + 1;

        int expected = n * (n + 1) / 2;

        int actual = 0;

        for (int value : arr) {
            actual += value;
        }

        System.out.println(expected - actual);
    }
}
```

---

## 27. Find duplicate elements in an array.

### Answer

```java
import java.util.HashSet;

public class Test {

    public static void main(String[] args) {

        int[] arr = {1, 2, 3, 2, 4, 1};

        HashSet<Integer> seen = new HashSet<>();
        HashSet<Integer> duplicates = new HashSet<>();

        for (int value : arr) {

            if (!seen.add(value)) {
                duplicates.add(value);
            }
        }

        System.out.println(duplicates);
    }
}
```

---

## 28. Move all zeroes to the end.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {0, 1, 0, 3, 12};

        int index = 0;

        for (int value : arr) {

            if (value != 0) {
                arr[index++] = value;
            }
        }

        while (index < arr.length) {
            arr[index++] = 0;
        }

        for (int value : arr) {
            System.out.print(value + " ");
        }
    }
}
```

Output:

```text
1 3 12 0 0
```

---

## 29. Remove duplicates from a sorted array.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {1, 1, 2, 2, 3, 4, 4};

        int index = 1;

        for (int i = 1; i < arr.length; i++) {

            if (arr[i] != arr[i - 1]) {
                arr[index++] = arr[i];
            }
        }

        for (int i = 0; i < index; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

This is a two-pointer pattern.

---

## 30. Rotate an array by one position to the right.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {1, 2, 3, 4, 5};

        int last = arr[arr.length - 1];

        for (int i = arr.length - 1; i > 0; i--) {
            arr[i] = arr[i - 1];
        }

        arr[0] = last;

        for (int value : arr) {
            System.out.print(value + " ");
        }
    }
}
```

Output:

```text
5 1 2 3 4
```

---

## 31. Find maximum subarray sum.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {-2, 1, -3, 4, -1, 2, 1, -5, 4};

        int current = arr[0];
        int max = arr[0];

        for (int i = 1; i < arr.length; i++) {

            current = Math.max(arr[i], current + arr[i]);
            max = Math.max(max, current);
        }

        System.out.println(max);
    }
}
```

This is Kadane's algorithm.

Time complexity:

```text
O(n)
```

---

## 32. Find intersection of two arrays.

### Answer

```java
import java.util.HashSet;

public class Test {

    public static void main(String[] args) {

        int[] a = {1, 2, 3, 4};
        int[] b = {3, 4, 5, 6};

        HashSet<Integer> set = new HashSet<>();

        for (int value : a) {
            set.add(value);
        }

        for (int value : b) {

            if (set.contains(value)) {
                System.out.print(value + " ");
            }
        }
    }
}
```

---

# 4. Searching

## 33. Implement linear search.

### Answer

```java
public class Test {

    static int linearSearch(int[] arr, int target) {

        for (int i = 0; i < arr.length; i++) {

            if (arr[i] == target) {
                return i;
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int[] arr = {10, 20, 30, 40};

        System.out.println(linearSearch(arr, 30));
    }
}
```

Time complexity:

```text
O(n)
```

---

## 34. Implement binary search.

### Answer

```java
public class Test {

    static int binarySearch(int[] arr, int target) {

        int left = 0;
        int right = arr.length - 1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (arr[mid] == target) {
                return mid;
            }

            if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        return -1;
    }

    public static void main(String[] args) {

        int[] arr = {10, 20, 30, 40, 50};

        System.out.println(binarySearch(arr, 40));
    }
}
```

Time complexity:

```text
O(log n)
```

Requirement:

```text
Array must be sorted.
```

---

## 35. Find first occurrence of an element in a sorted array.

### Answer

```java
public class Test {

    static int firstOccurrence(int[] arr, int target) {

        int left = 0;
        int right = arr.length - 1;
        int answer = -1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (arr[mid] == target) {

                answer = mid;
                right = mid - 1;

            } else if (arr[mid] < target) {

                left = mid + 1;

            } else {

                right = mid - 1;
            }
        }

        return answer;
    }

    public static void main(String[] args) {

        int[] arr = {1, 2, 2, 2, 3, 4};

        System.out.println(firstOccurrence(arr, 2));
    }
}
```

---

## 36. Find last occurrence of an element.

### Answer

```java
public class Test {

    static int lastOccurrence(int[] arr, int target) {

        int left = 0;
        int right = arr.length - 1;
        int answer = -1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (arr[mid] == target) {

                answer = mid;
                left = mid + 1;

            } else if (arr[mid] < target) {

                left = mid + 1;

            } else {

                right = mid - 1;
            }
        }

        return answer;
    }

    public static void main(String[] args) {

        int[] arr = {1, 2, 2, 2, 3, 4};

        System.out.println(lastOccurrence(arr, 2));
    }
}
```

---

# 5. Sorting

## 37. Implement bubble sort.

### Answer

```java
public class Test {

    static void bubbleSort(int[] arr) {

        for (int i = 0; i < arr.length - 1; i++) {

            boolean swapped = false;

            for (int j = 0; j < arr.length - 1 - i; j++) {

                if (arr[j] > arr[j + 1]) {

                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;

                    swapped = true;
                }
            }

            if (!swapped) {
                break;
            }
        }
    }

    public static void main(String[] args) {

        int[] arr = {5, 3, 1, 4, 2};

        bubbleSort(arr);

        for (int value : arr) {
            System.out.print(value + " ");
        }
    }
}
```

Average/worst-case complexity:

```text
O(n²)
```

---

## 38. Implement selection sort.

### Answer

```java
public class Test {

    static void selectionSort(int[] arr) {

        for (int i = 0; i < arr.length - 1; i++) {

            int minIndex = i;

            for (int j = i + 1; j < arr.length; j++) {

                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }

            int temp = arr[i];
            arr[i] = arr[minIndex];
            arr[minIndex] = temp;
        }
    }

    public static void main(String[] args) {

        int[] arr = {5, 3, 1, 4, 2};

        selectionSort(arr);

        for (int value : arr) {
            System.out.print(value + " ");
        }
    }
}
```

Time complexity:

```text
O(n²)
```

---

## 39. Implement insertion sort.

### Answer

```java
public class Test {

    static void insertionSort(int[] arr) {

        for (int i = 1; i < arr.length; i++) {

            int key = arr[i];
            int j = i - 1;

            while (j >= 0 && arr[j] > key) {

                arr[j + 1] = arr[j];
                j--;
            }

            arr[j + 1] = key;
        }
    }

    public static void main(String[] args) {

        int[] arr = {5, 3, 1, 4, 2};

        insertionSort(arr);

        for (int value : arr) {
            System.out.print(value + " ");
        }
    }
}
```

Worst-case complexity:

```text
O(n²)
```

---

# 6. Collections

## 40. Find frequency of elements using HashMap.

### Answer

```java
import java.util.HashMap;
import java.util.Map;

public class Test {

    public static void main(String[] args) {

        int[] arr = {1, 2, 2, 3, 1, 1};

        Map<Integer, Integer> map = new HashMap<>();

        for (int value : arr) {
            map.put(value, map.getOrDefault(value, 0) + 1);
        }

        System.out.println(map);
    }
}
```

---

## 41. Find the most frequent element.

### Answer

```java
import java.util.HashMap;
import java.util.Map;

public class Test {

    public static void main(String[] args) {

        int[] arr = {1, 2, 2, 3, 1, 1};

        Map<Integer, Integer> map = new HashMap<>();

        for (int value : arr) {
            map.put(value, map.getOrDefault(value, 0) + 1);
        }

        int answer = arr[0];

        for (int value : map.keySet()) {

            if (map.get(value) > map.get(answer)) {
                answer = value;
            }
        }

        System.out.println(answer);
    }
}
```

---

## 42. Remove duplicates from an array.

### Answer

```java
import java.util.LinkedHashSet;

public class Test {

    public static void main(String[] args) {

        int[] arr = {1, 2, 2, 3, 1, 4};

        LinkedHashSet<Integer> set = new LinkedHashSet<>();

        for (int value : arr) {
            set.add(value);
        }

        System.out.println(set);
    }
}
```

---

## 43. Check whether an array contains duplicates.

### Answer

```java
import java.util.HashSet;

public class Test {

    public static void main(String[] args) {

        int[] arr = {1, 2, 3, 4, 2};

        HashSet<Integer> set = new HashSet<>();

        boolean duplicate = false;

        for (int value : arr) {

            if (!set.add(value)) {
                duplicate = true;
                break;
            }
        }

        System.out.println(duplicate);
    }
}
```

---

## 44. Find common elements between two lists.

### Answer

```java
import java.util.ArrayList;
import java.util.HashSet;

public class Test {

    public static void main(String[] args) {

        ArrayList<Integer> a = new ArrayList<>();
        a.add(1);
        a.add(2);
        a.add(3);

        ArrayList<Integer> b = new ArrayList<>();
        b.add(2);
        b.add(3);
        b.add(4);

        HashSet<Integer> set = new HashSet<>(a);

        for (int value : b) {

            if (set.contains(value)) {
                System.out.println(value);
            }
        }
    }
}
```

---

# 7. Recursion

## 45. Calculate factorial using recursion.

### Answer

```java
public class Test {

    static int factorial(int n) {

        if (n == 0 || n == 1) {
            return 1;
        }

        return n * factorial(n - 1);
    }

    public static void main(String[] args) {

        System.out.println(factorial(5));
    }
}
```

---

## 46. Calculate Fibonacci using recursion.

### Answer

```java
public class Test {

    static int fibonacci(int n) {

        if (n <= 1) {
            return n;
        }

        return fibonacci(n - 1) + fibonacci(n - 2);
    }

    public static void main(String[] args) {

        System.out.println(fibonacci(6));
    }
}
```

The simple recursive approach has exponential time complexity.

---

## 47. Reverse a String using recursion.

### Answer

```java
public class Test {

    static String reverse(String s) {

        if (s.length() <= 1) {
            return s;
        }

        return reverse(s.substring(1)) + s.charAt(0);
    }

    public static void main(String[] args) {

        System.out.println(reverse("Java"));
    }
}
```

---

## 48. Calculate sum of first `n` natural numbers using recursion.

### Answer

```java
public class Test {

    static int sum(int n) {

        if (n == 0) {
            return 0;
        }

        return n + sum(n - 1);
    }

    public static void main(String[] args) {

        System.out.println(sum(5));
    }
}
```

---

# 8. Patterns and Logic

## 49. Find whether two arrays contain the same elements.

### Answer

```java
import java.util.Arrays;

public class Test {

    public static void main(String[] args) {

        int[] a = {1, 2, 3};
        int[] b = {3, 1, 2};

        Arrays.sort(a);
        Arrays.sort(b);

        System.out.println(Arrays.equals(a, b));
    }
}
```

---

## 50. Find the maximum and minimum in one traversal.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {5, 2, 9, 1, 7};

        int min = arr[0];
        int max = arr[0];

        for (int i = 1; i < arr.length; i++) {

            min = Math.min(min, arr[i]);
            max = Math.max(max, arr[i]);
        }

        System.out.println("Min = " + min);
        System.out.println("Max = " + max);
    }
}
```

Time complexity:

```text
O(n)
```

---

## 51. Find pair with a given sum.

### Answer

```java
import java.util.HashSet;

public class Test {

    public static void main(String[] args) {

        int[] arr = {2, 7, 11, 15};
        int target = 9;

        HashSet<Integer> set = new HashSet<>();

        for (int value : arr) {

            int required = target - value;

            if (set.contains(required)) {

                System.out.println(required + " " + value);
                break;
            }

            set.add(value);
        }
    }
}
```

Time complexity:

```text
O(n)
```

Average extra space:

```text
O(n)
```

---

## 52. Find duplicate number using a HashSet.

### Answer

```java
import java.util.HashSet;

public class Test {

    public static void main(String[] args) {

        int[] arr = {1, 3, 4, 2, 2};

        HashSet<Integer> set = new HashSet<>();

        for (int value : arr) {

            if (!set.add(value)) {
                System.out.println(value);
                break;
            }
        }
    }
}
```

---

## 53. Count even and odd numbers in an array.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {1, 2, 3, 4, 5, 6};

        int even = 0;
        int odd = 0;

        for (int value : arr) {

            if (value % 2 == 0) {
                even++;
            } else {
                odd++;
            }
        }

        System.out.println("Even = " + even);
        System.out.println("Odd = " + odd);
    }
}
```

---

## 54. Find all prime numbers from 1 to `n`.

### Answer

```java
public class Test {

    static boolean isPrime(int n) {

        if (n < 2) {
            return false;
        }

        for (int i = 2; i * i <= n; i++) {

            if (n % i == 0) {
                return false;
            }
        }

        return true;
    }

    public static void main(String[] args) {

        int n = 30;

        for (int i = 2; i <= n; i++) {

            if (isPrime(i)) {
                System.out.print(i + " ");
            }
        }
    }
}
```

---

## 55. Find the second smallest element.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {5, 2, 8, 1, 4};

        int smallest = Integer.MAX_VALUE;
        int second = Integer.MAX_VALUE;

        for (int value : arr) {

            if (value < smallest) {

                second = smallest;
                smallest = value;

            } else if (value < second && value != smallest) {

                second = value;
            }
        }

        System.out.println(second);
    }
}
```

---

# 9. Common Interview Coding Questions

## 56. Implement two-sum.

### Answer

```java
import java.util.HashMap;

public class Test {

    static int[] twoSum(int[] arr, int target) {

        HashMap<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < arr.length; i++) {

            int required = target - arr[i];

            if (map.containsKey(required)) {
                return new int[]{map.get(required), i};
            }

            map.put(arr[i], i);
        }

        return new int[]{-1, -1};
    }

    public static void main(String[] args) {

        int[] arr = {2, 7, 11, 15};

        int[] result = twoSum(arr, 9);

        System.out.println(result[0] + " " + result[1]);
    }
}
```

Time complexity:

```text
O(n)
```

---

## 57. Implement two-sum using two pointers.

### Answer

```java
import java.util.Arrays;

public class Test {

    public static void main(String[] args) {

        int[] arr = {2, 7, 11, 15};
        int target = 9;

        Arrays.sort(arr);

        int left = 0;
        int right = arr.length - 1;

        while (left < right) {

            int sum = arr[left] + arr[right];

            if (sum == target) {

                System.out.println(arr[left] + " " + arr[right]);
                break;

            } else if (sum < target) {

                left++;

            } else {

                right--;
            }
        }
    }
}
```

Two pointers require a sorted array.

---

## 58. Find the maximum profit from stock prices.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] prices = {7, 1, 5, 3, 6, 4};

        int minPrice = prices[0];
        int maxProfit = 0;

        for (int price : prices) {

            minPrice = Math.min(minPrice, price);

            maxProfit = Math.max(
                    maxProfit,
                    price - minPrice
            );
        }

        System.out.println(maxProfit);
    }
}
```

Time complexity:

```text
O(n)
```

---

## 59. Find the longest substring without repeating characters.

### Answer

```java
import java.util.HashSet;

public class Test {

    public static void main(String[] args) {

        String s = "abcabcbb";

        HashSet<Character> set = new HashSet<>();

        int left = 0;
        int maxLength = 0;

        for (int right = 0; right < s.length(); right++) {

            while (set.contains(s.charAt(right))) {
                set.remove(s.charAt(left));
                left++;
            }

            set.add(s.charAt(right));

            maxLength = Math.max(
                    maxLength,
                    right - left + 1
            );
        }

        System.out.println(maxLength);
    }
}
```

This uses the sliding-window technique.

---

## 60. Find minimum size subarray sum greater than or equal to target.

### Answer

```java
public class Test {

    static int minSubArrayLen(int target, int[] nums) {

        int left = 0;
        int sum = 0;
        int answer = Integer.MAX_VALUE;

        for (int right = 0; right < nums.length; right++) {

            sum += nums[right];

            while (sum >= target) {

                answer = Math.min(
                        answer,
                        right - left + 1
                );

                sum -= nums[left];
                left++;
            }
        }

        return answer == Integer.MAX_VALUE ? 0 : answer;
    }

    public static void main(String[] args) {

        int[] nums = {2, 3, 1, 2, 4, 3};

        System.out.println(minSubArrayLen(7, nums));
    }
}
```

Output:

```text
2
```

The subarray is:

```text
4, 3
```

---

## 61. Merge two sorted arrays.

### Answer

```java
public class Test {

    static int[] merge(int[] a, int[] b) {

        int[] result = new int[a.length + b.length];

        int i = 0;
        int j = 0;
        int k = 0;

        while (i < a.length && j < b.length) {

            if (a[i] <= b[j]) {
                result[k++] = a[i++];
            } else {
                result[k++] = b[j++];
            }
        }

        while (i < a.length) {
            result[k++] = a[i++];
        }

        while (j < b.length) {
            result[k++] = b[j++];
        }

        return result;
    }

    public static void main(String[] args) {

        int[] a = {1, 3, 5};
        int[] b = {2, 4, 6};

        int[] result = merge(a, b);

        for (int value : result) {
            System.out.print(value + " ");
        }
    }
}
```

Time complexity:

```text
O(n + m)
```

---

## 62. Check balanced parentheses.

### Answer

```java
import java.util.Stack;

public class Test {

    static boolean isBalanced(String s) {

        Stack<Character> stack = new Stack<>();

        for (char ch : s.toCharArray()) {

            if (ch == '(' ||
                ch == '[' ||
                ch == '{') {

                stack.push(ch);

            } else {

                if (stack.isEmpty()) {
                    return false;
                }

                char top = stack.pop();

                if ((ch == ')' && top != '(') ||
                    (ch == ']' && top != '[') ||
                    (ch == '}' && top != '{')) {

                    return false;
                }
            }
        }

        return stack.isEmpty();
    }

    public static void main(String[] args) {

        System.out.println(
                isBalanced("{[()]}")
        );
    }
}
```

---

## 63. Find intersection of two sorted arrays.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] a = {1, 2, 3, 4, 5};
        int[] b = {2, 4, 5, 6};

        int i = 0;
        int j = 0;

        while (i < a.length && j < b.length) {

            if (a[i] == b[j]) {

                System.out.print(a[i] + " ");
                i++;
                j++;

            } else if (a[i] < b[j]) {

                i++;

            } else {

                j++;
            }
        }
    }
}
```

Time complexity:

```text
O(n + m)
```

---

## 64. Rotate an array by `k` positions.

### Answer

```java
public class Test {

    static void reverse(int[] arr, int left, int right) {

        while (left < right) {

            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;

            left++;
            right--;
        }
    }

    static void rotate(int[] arr, int k) {

        k %= arr.length;

        reverse(arr, 0, arr.length - 1);
        reverse(arr, 0, k - 1);
        reverse(arr, k, arr.length - 1);
    }

    public static void main(String[] args) {

        int[] arr = {1, 2, 3, 4, 5};
        int k = 2;

        rotate(arr, k);

        for (int value : arr) {
            System.out.print(value + " ");
        }
    }
}
```

Output:

```text
4 5 1 2 3
```

---

## 65. Find majority element.

### Answer

```java
public class Test {

    static int majorityElement(int[] arr) {

        int candidate = 0;
        int count = 0;

        for (int value : arr) {

            if (count == 0) {
                candidate = value;
            }

            count += value == candidate ? 1 : -1;
        }

        return candidate;
    }

    public static void main(String[] args) {

        int[] arr = {2, 2, 1, 1, 1, 2, 2};

        System.out.println(majorityElement(arr));
    }
}
```

This is Boyer-Moore voting.

The algorithm assumes a majority element exists.

---

## 66. Find the maximum consecutive ones.

### Answer

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = {1, 1, 0, 1, 1, 1};

        int current = 0;
        int maximum = 0;

        for (int value : arr) {

            if (value == 1) {

                current++;
                maximum = Math.max(maximum, current);

            } else {

                current = 0;
            }
        }

        System.out.println(maximum);
    }
}
```

---

# 10. Rapid-Fire Coding Revision

## 67. Reverse an array

```text
Two pointers
```

---

## 68. Reverse a String

```text
StringBuilder.reverse()
```

---

## 69. Find largest element

```text
Single traversal
```

---

## 70. Find smallest element

```text
Single traversal
```

---

## 71. Find missing number from `1..n`

```text
Sum formula
or
XOR
```

---

## 72. Detect duplicate

```text
HashSet
```

---

## 73. Frequency counting

```text
HashMap
```

---

## 74. Pair sum

```text
HashMap / HashSet
```

---

## 75. Pair sum in sorted array

```text
Two pointers
```

---

## 76. Maximum subarray sum

```text
Kadane's Algorithm
```

---

## 77. Maximum stock profit

```text
Track minimum price + maximum profit
```

---

## 78. Binary search

```text
Sorted data + divide search space
```

---

## 79. First occurrence

```text
Binary search + move right boundary left
```

---

## 80. Last occurrence

```text
Binary search + move left boundary right
```

---

## 81. Remove duplicates from sorted array

```text
Two pointers
```

---

## 82. Move zeroes

```text
Two pointers / write pointer
```

---

## 83. Longest substring without repeating characters

```text
Sliding window + HashSet/HashMap
```

---

## 84. Minimum size subarray sum

```text
Sliding window
```

---

## 85. Merge sorted arrays

```text
Two pointers
```

---

## 86. Balanced parentheses

```text
Stack
```

---

## 87. Majority element

```text
Boyer-Moore Voting Algorithm
```

---

## 88. Prime checking

```text
Check divisors up to sqrt(n)
```

---

## 89. Factorial

```text
Loop or recursion
```

---

## 90. Fibonacci

```text
Iterative approach preferred over naive recursion
```

---

## 91. Palindrome

```text
Compare from both ends
```

---

## 92. Anagram

```text
Sorting
or
frequency counting
```

---

## 93. First non-repeating character

```text
Frequency map + second traversal
```

---

## 94. Duplicate characters

```text
Set
```

---

## 95. Character frequency

```text
HashMap<Character, Integer>
```

---

## 96. Array maximum in one pass

```text
O(n)
```

---

## 97. Binary search complexity

```text
O(log n)
```

---

## 98. HashMap lookup average complexity

```text
O(1)
```

---

## 99. HashSet lookup average complexity

```text
O(1)
```

---

## 100. Core interview rule

```text
First understand the brute-force solution.

Then identify:
- repeated work
- unnecessary traversal
- sorted data
- frequency/count requirement
- contiguous subarray/substring
- pair/triplet relationship
- need for ordering
- possibility of two pointers
- possibility of sliding window
- possibility of hashing

Then optimize time and space.
```

# 🧠 Coding Interview Checklist

```text
1. Understand the input.

2. Understand the expected output.

3. Check edge cases.

4. Write brute force first.

5. Calculate time complexity.

6. Calculate space complexity.

7. Look for a better pattern.

8. Test with a small example.

9. Test empty/single-element input where applicable.

10. Test duplicate values.

11. Test negative values where applicable.

12. Test already sorted input.

13. Test the largest/smallest relevant values.

14. Explain the approach before writing code.

15. After coding, dry-run the code manually.
```


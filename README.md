LeetCode Problem Solutions in Java

This repository contains solutions to 16 LeetCode problems assigned as part of the CAP392 course.

Programming Language: Java

Difficulty Levels: Easy, Medium, and Hard

Total Problems: 16

Easy

7_Reverse_Integer.java
code-class Solution {
    public int reverse(int x) {
        long reversed = 0;

        while (x != 0) {
            int digit = x % 10;
            reversed = reversed * 10 + digit;

            if (reversed > Integer.MAX_VALUE ||
                reversed < Integer.MIN_VALUE) {
                return 0;
            }

            x /= 10;
        }

        return (int) reversed;
    }
}

9_Palindrome_Number.java
code-class Solution {
    public boolean isPalindrome(int x) {

        // Negative numbers are not palindrome
        if (x < 0) {
            return false;
        }

        int original = x;
        int reverse = 0;

        while (x > 0) {
            int digit = x % 10;

            reverse = reverse * 10 + digit;

            x = x / 10;
        }

        return original == reverse;
    }
}


13_Roman_to_Integer.java
code-import java.util.HashMap;

class Solution {
    public int romanToInt(String s) {

        HashMap<Character, Integer> map = new HashMap<>();

        map.put('I', 1);
        map.put('V', 5);
        map.put('X', 10);
        map.put('L', 50);
        map.put('C', 100);
        map.put('D', 500);
        map.put('M', 1000);

        int result = 0;

        for (int i = 0; i < s.length(); i++) {

            int current = map.get(s.charAt(i));

            // Check next character
            if (i < s.length() - 1 &&
                current < map.get(s.charAt(i + 1))) {

                result = result - current;

            } else {

                result = result + current;
            }
        }

        return result;
    }
}


14_Longest_Common_Prefix.java
code-class Solution {
    public String longestCommonPrefix(String[] strs) {

        String prefix = strs[0];

        for (int i = 1; i < strs.length; i++) {

            while (!strs[i].startsWith(prefix)) {

                prefix = prefix.substring(0, prefix.length() - 1);

                if (prefix.isEmpty()) {
                    return "";
                }
            }
        }

        return prefix;
    }
}


27_Remove_Element.java
code-class Solution {
    public int removeElement(int[] nums, int val) {

        int k = 0;

        for (int i = 0; i < nums.length; i++) {

            if (nums[i] != val) {
                nums[k] = nums[i];
                k++;
            }
        }

        return k;
    }
}


35_Search_Insert_Position.java
code-class Solution {
    public int searchInsert(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {
                return mid;
            }
            else if (nums[mid] < target) {
                left = mid + 1;
            }
            else {
                right = mid - 1;
            }
        }

        return left;
    }
}


58_Length_of_Last_Word.java
code-class Solution {
    public int lengthOfLastWord(String s) {

        int i = s.length() - 1;

        // Skip trailing spaces
        while (i >= 0 && s.charAt(i) == ' ') {
            i--;
        }

        int count = 0;

        // Count characters of the last word
        while (i >= 0 && s.charAt(i) != ' ') {
            count++;
            i--;
        }

        return count;
    }
}


66_Plus_One.java
code-class Solution {
    public int[] plusOne(int[] digits) {

        for (int i = digits.length - 1; i >= 0; i--) {

            if (digits[i] < 9) {
                digits[i]++;
                return digits;
            }

            digits[i] = 0;
        }

        // If all digits are 9, increase array size
        int[] result = new int[digits.length + 1];
        result[0] = 1;

        return result;
    }
}


125_Valid_Palindrome.java
code-class Solution {
    public boolean isPalindrome(String s) {

        int left = 0;
        int right = s.length() - 1;

        while (left < right) {

            // Skip non-alphanumeric characters from left
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }

            // Skip non-alphanumeric characters from right
            while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }

            // Compare characters ignoring uppercase/lowercase
            if (Character.toLowerCase(s.charAt(left)) !=
                Character.toLowerCase(s.charAt(right))) {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }
}



Medium
56_Merge_Intervals.java
code-import java.util.Arrays;
import java.util.ArrayList;
import java.util.List;

class Solution {
    public int[][] merge(int[][] intervals) {

        // Step 1: Sort intervals based on starting time
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

        List<int[]> result = new ArrayList<>();

        // Step 2: Add the first interval
        int start = intervals[0][0];
        int end = intervals[0][1];

        // Step 3: Traverse remaining intervals
        for (int i = 1; i < intervals.length; i++) {

            if (intervals[i][0] <= end) {
                // Overlapping intervals: merge them
                end = Math.max(end, intervals[i][1]);
            } else {
                // No overlap: add previous interval
                result.add(new int[]{start, end});

                start = intervals[i][0];
                end = intervals[i][1];
            }
        }

        // Add the last interval
        result.add(new int[]{start, end});

        return result.toArray(new int[result.size()][]);
    }
}


189_Rotate_Array.java
code-class Solution {
    public void rotate(int[] nums, int k) {

        int n = nums.length;

        // Handle k greater than array length
        k = k % n;

        // Reverse the entire array
        reverse(nums, 0, n - 1);

        // Reverse first k elements
        reverse(nums, 0, k - 1);

        // Reverse remaining elements
        reverse(nums, k, n - 1);
    }

    public void reverse(int[] nums, int left, int right) {

        while (left < right) {

            int temp = nums[left];
            nums[left] = nums[right];
            nums[right] = temp;

            left++;
            right--;
        }
    }
}


31_Next_Permutation.java
code-class Solution {
    public void nextPermutation(int[] nums) {
        int n = nums.length;
        int i = n - 2;

        // Step 1: Find the first decreasing element from right
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            i--;
        }

        // Step 2: Find the smallest element greater than nums[i]
        if (i >= 0) {
            int j = n - 1;

            while (nums[j] <= nums[i]) {
                j--;
            }

            // Step 3: Swap both elements
            swap(nums, i, j);
        }

        // Step 4: Reverse the elements after index i
        reverse(nums, i + 1, n - 1);
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }

    private void reverse(int[] nums, int start, int end) {
        while (start < end) {
            swap(nums, start, end);
            start++;
            end--;
        }
    }
}

54_Spiral_Matrix.java
code-import java.util.*;

class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<>();

        int top = 0;
        int bottom = matrix.length - 1;
        int left = 0;
        int right = matrix[0].length - 1;

        while (top <= bottom && left <= right) {

            // 1. Traverse left to right
            for (int i = left; i <= right; i++) {
                result.add(matrix[top][i]);
            }
            top++;

            // 2. Traverse top to bottom
            for (int i = top; i <= bottom; i++) {
                result.add(matrix[i][right]);
            }
            right--;

            // 3. Traverse right to left
            if (top <= bottom) {
                for (int i = right; i >= left; i--) {
                    result.add(matrix[bottom][i]);
                }
                bottom--;
            }

            // 4. Traverse bottom to top
            if (left <= right) {
                for (int i = bottom; i >= top; i--) {
                    result.add(matrix[i][left]);
                }
                left++;
            }
        }

        return result;
    }
}

73_Set_Matrix_Zeroes.java
code-class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;

        boolean firstRow = false;
        boolean firstCol = false;

        // Step 1: Check if the first row contains zero
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) {
                firstRow = true;
                break;
            }
        }

        // Step 2: Check if the first column contains zero
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) {
                firstCol = true;
                break;
            }
        }

        // Step 3: Mark rows and columns using first row/column
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        // Step 4: Set matrix elements to zero using markers
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }

        // Step 5: Zero the first row if needed
        if (firstRow) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }

        // Step 6: Zero the first column if needed
        if (firstCol) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }
    }
}



Hard
65_Valid_Number.java
code-lass Solution {
    public boolean isNumber(String s) {
        boolean digit = false;
        boolean dot = false;
        boolean exp = false;

        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);

            if (Character.isDigit(ch)) {
                digit = true;
            }
            else if (ch == '+' || ch == '-') {
                // Sign is allowed only at the beginning
                // or immediately after e/E
                if (i != 0 && s.charAt(i - 1) != 'e'
                           && s.charAt(i - 1) != 'E') {
                    return false;
                }
                digit = false;
            }
            else if (ch == '.') {
                // Dot is not allowed after exponent
                // or if another dot already exists
                if (dot || exp) {
                    return false;
                }
                dot = true;
            }
            else if (ch == 'e' || ch == 'E') {
                // Exponent requires a digit before it
                // and can appear only once
                if (exp || !digit) {
                    return false;
                }
                exp = true;
                digit = false;
            }
            else {
                return false;
            }
        }

        return digit;
    }
}


41_First_Missing_Positive.java
code-class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;

        // Step 1: Place each number at its correct index
        for (int i = 0; i < n; i++) {

            while (nums[i] > 0 &&
                   nums[i] <= n &&
                   nums[nums[i] - 1] != nums[i]) {

                int correctIndex = nums[i] - 1;

                // Swap current number to correct position
                int temp = nums[i];
                nums[i] = nums[correctIndex];
                nums[correctIndex] = temp;
            }
        }

        // Step 2: Find the first missing positive number
        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }

        // Step 3: All numbers from 1 to n are present
        return n + 1;
    }
}



| ------- |
| [0007-reverse-integer](https://github.com/priyanshuchoubey357-spec/LeetCode-Solutions/tree/master/0007-reverse-integer) |
<!---LeetCode Topics End-->

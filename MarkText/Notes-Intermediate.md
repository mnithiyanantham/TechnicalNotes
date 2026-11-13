#### Longest Palindromic Substring

**Problem Description**  

Given a string A of size N, find and return the **longest palindromic substring** in A.

Substring of string A is `A[i...j]` where 0 <= i <= j < len(A)

**Palindrome string:**  A string which reads the same backwards. More formally, A is palindrome if reverse(A) = A.

**Incase of conflict**, return the substring which occurs first ( with the least starting index).

**Psedocode:**

* Use center expansion Technique

```java
public class Solution {

    public static String longestPalindrome(String A) {
        int max = 0;
		int index = 0;
        
		if (A.length() == 0)
			return "";

		for (int i = 0; i < A.length(); i++) {
			// odd length
			int j = i - 1; // keep center odd index
			int k = i + 1;
			while (j >= 0 && k < A.length() && A.charAt(j) == A.charAt(k)) {
				j--;
				k++;
			}
			if (max < k - j - 1) {
				index = i;
				max = k - j - 1;
			}

			// even length
			j = i; // keep center as even index
			k = i + 1;
			while (j >= 0 && k < A.length() && A.charAt(j) == A.charAt(k)) {
				j--;
				k++;
			}
			if (max < k - j - 1) {
                index = i;
				max = k - j - 1;
			}
		}

		String ans;
        //System.out.println("max: " + max + " index:" + index);

		if (max % 2 == 0) {
			ans = A.substring(index-(max/2)+1, index+(max/2)+1);
		} else {
			ans = A.substring(index-(max/2), index+(max/2)+1);
		}
		return ans;
    }

    private int getMax(int max, int i, int j) {
        if(max < i-j-1) {
            return i-j-1;
        } else {
            return max;
        }
    }
    // ajmadam 4 2
    // maddam 2 
}
```

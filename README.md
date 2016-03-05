# CSCI223MidtermJamesAndrus

##Problem 2
```
public class FindDuplicate {

	public static void main(String[] args) {
		int[] ints = new int[100];
		// fill array with integers
		for (int i = 0; i < ints.length; i++) {
			ints[i] = i + 1;
		}
		ints[98] = 98;
		for (int i = 0; i < ints.length; i++) {
			System.out.println(ints[i]);
		}
		findDuplicate(ints);
	}

	//method to make recursive call with safety net for the length of the called array
	public static void findDuplicate(int[] a) {
		if (a.length <= 1) {
			System.out.println("No duplicates");
		}
		findDuplicate(a, 0, a.length - 1);
	}

	//recursive method to find duplicate
	private static void findDuplicate(int[] a, int lo, int hi) {
		int mid = (lo + (hi - lo) / 2);
		if (hi <= lo) {
			return;
		}
		if (a[mid] == a[hi] || a[mid] == a[mid + 1]) {
			System.out.println("duplicate: " + a[mid]);
			return;
		}
		findDuplicate(a, lo, mid);
		findDuplicate(a, mid + 1, hi);
	}
}
```
##Problem 3
###Modified partition on unshuffled array
```
	private static int partition(Comparable[] a, int lo, int hi) {
		int i = lo;
		int j = hi + 1;
		// exchange element at lo with random element
		int randomElement = (lo + (int) (Math.random() * ((hi - lo) + 1)));
		exch(a, lo, randomElement);
		Comparable v = a[lo];
		while (true) {
			// find item on lo to swap
			while (less(a[++i], v))
				if (i == hi)
					break;
			// find item on hi to swap
			while (less(v, a[--j]))
				if (j == lo)
					break; // redundant since a[lo] acts as sentinel
			// check if pointers cross
			if (i >= j)
				break;
			exch(a, i, j);
		}
		// put partitioning item v at a[j]
		exch(a, lo, j);
		// now, a[lo .. j-1] <= a[j] <= a[j+1 .. hi]
		return j;
	}
```
###Regular partion for shuffled array
```
    private static int partition(Comparable[] a, int lo, int hi) {
        int i = lo;
        int j = hi + 1;
        Comparable v = a[lo];
        while (true) { 
            // find item on lo to swap
            while (less(a[++i], v))
                if (i == hi) break;
            // find item on hi to swap
            while (less(v, a[--j]))
                if (j == lo) break;      // redundant since a[lo] acts as sentinel
            // check if pointers cross
            if (i >= j) break;
            exch(a, i, j);
        }
        // put partitioning item v at a[j]
        exch(a, lo, j);
        // now, a[lo .. j-1] <= a[j] <= a[j+1 .. hi]
        return j;
    }
```
###Tester for Problem 3
```
public class MidTermProblem3 {

	public static void main(String[] args) {
		Integer[] ints = new Integer[10000];
		// fill array with integers
		for (int i = 0; i < ints.length; i++) {
			ints[i] = i + 1;
		}
		int counter = 0;
		int totalCompares = 0;
		// while loop to repeat experiment
		while (counter <= 10000) {
			Quick.sort(ints);
			int k = Quick.getCompares();
			totalCompares += k;
			counter++;
		}
		int avg = totalCompares / counter;
		System.out.println("Average with QuickSort: " + avg);
		int counter1 = 0;
		int totalCompares1 = 0;
		// while loop to repeat experiment
		while (counter1 <= 10000) {
			Quick.sort(ints);
			int k = Quick.getCompares();
			totalCompares1 += k;
			counter1++;
		}
		int avg1 = totalCompares1 / counter1;
		System.out.println("Average with Modified QuickSort: " + avg1);
		System.out.println("Difference: " + (avg - avg1));
	}
}
```

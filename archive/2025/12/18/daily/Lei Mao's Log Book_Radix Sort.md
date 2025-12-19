---
title: Radix Sort
url: https://leimao.github.io/blog/Radix-Sort/
source: Lei Mao's Log Book
date: 2025-12-18
fetch_date: 2025-12-19T03:25:55.199427
---

# Radix Sort

[![Lei Mao's Log Book](/images/favicon/android-chrome-512x512.png)](/)

[Lei Mao's Log Book](/)[Curriculum](/curriculum)[Blog](/blog)[Articles](/article)[Projects](/project)[Publications](/publication)[Readings](/reading)[Life](/life)[Essay](/essay)[Photography](/photography)[Archives](/archives)[Categories](/categories)[Tags](/tags)[FAQs](/faq)

# Radix Sort

12-18-202512-18-2025 [blog](/blog/) 19 minutes read (About 2808 words)  visits

## Introduction

Radix sort is a non-comparative sorting algorithm that sorts numbers by processing individual digits. Radix means the “base” of a number system. For example, the decimal number system has a radix of 10, while the binary number system has a radix of 2. It works by sorting the input numbers based on each digit, starting from the least significant digit (LSD) to the most significant digit (MSD) or vice versa. Radix sort typically uses counting sort as a subroutine to sort the digits.

In this blog post, I would like to quickly discuss the radix sort algorithm and present a few implementations in Python and C++.

## Counting Sort for Integer Numbers

The idea of counting sort is straightforward. We count the occurrences of each unique element in the input array and use this information to place each element directly into its correct position in the output array. Counting sort is efficient when the range of input values is not significantly larger than the number of elements to be sorted.

For example, consider the following array of integers:

|  |  |
| --- | --- |
| ``` 1 ``` | ``` input_array = [4, 2, 2, 8, 3, 3, 1] ``` |

We can create a count array to store the frequency of each integer:

|  |  |
| --- | --- |
| ``` 1 ``` | ``` count = [0, 1, 2, 2, 1, 0, 0, 0, 1]  # Index represents the integer value ``` |

Next, we modify the count array to store the cumulative counts instead:

|  |  |
| --- | --- |
| ``` 1 ``` | ``` count = [0, 1, 3, 5, 6, 6, 6, 6, 7] ``` |

Finally, we build the output array by placing each element in its correct position based on the count array. This can be done iteratively.

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 ``` | ``` output_array = [-1, -1, -1, -1, -1, -1, -1] # Initialize output array # Reverse iterate to maintain the stability of the sort, i.e., keep the relative order of equal elements. for num in reversed(input_array):     output_array[count[num] - 1] = num     count[num] -= 1 # output_array = [1, 2, 2, 3, 3, 4, 8] ``` |

### Python Implementation

The counting sort algorithm can be implemented using Python as follows.

counting\_sort.py

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 ``` | ``` def counting_sort(arr):      if not arr:         return arr      max_val = max(arr)     min_val = min(arr)     range_of_elements = max_val - min_val + 1      # Initialize count array     count = [0] * range_of_elements     output = [0] * len(arr)      # Store the count of each element     for num in arr:         count[num - min_val] += 1      # Change count[i] so that it contains the actual position of this element in output array     for i in range(1, len(count)):         count[i] += count[i - 1]      # Build the output array     for num in reversed(arr):         output[count[num - min_val] - 1] = num         count[num - min_val] -= 1      return output  if __name__ == "__main__":      input_array = [4, 2, 2, 8, 3, 3, 1]     sorted_array = counting_sort(input_array)      print("Array to be sorted:", input_array)     print("Sorted array:", sorted_array) ``` |

|  |  |
| --- | --- |
| ``` 1 2 3 ``` | ``` $ python counting_sort.py Array to be sorted: [4, 2, 2, 8, 3, 3, 1] Sorted array: [1, 2, 2, 3, 3, 4, 8] ``` |

### C++ Implementation

counting\_sort.cpp

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 ``` | ``` #include <algorithm> #include <iostream> #include <vector>  std::vector<int> counting_sort(std::vector<int> const& arr) {     if (arr.empty())     {         return arr;     }      int max_val{*std::max_element(arr.begin(), arr.end())};     int min_val{*std::min_element(arr.begin(), arr.end())};     int range_of_elements{max_val - min_val + 1};      // Initialize count array     std::vector<int> count(range_of_elements, 0);     std::vector<int> output(arr.size());      // Store the count of each element     for (int const& num : arr)     {         count[num - min_val]++;     }      // Change count[i] so that it contains the actual position of this element     // in output array     for (size_t i{1}; i < count.size(); i++)     {         count[i] += count[i - 1];     }      // Build the output array     for (size_t i{arr.size()}; i > 0; i--)     {         int num{arr[i - 1]};         output[count[num - min_val] - 1] = num;         count[num - min_val]--;     }      return output; }  int main() {     std::vector<int> const input_array{4, 2, 2, 8, 3, 3, 1};     std::vector<int> const sorted_array{counting_sort(input_array)};      std::cout << "Array to be sorted: ";     for (int const& num : input_array)     {         std::cout << num << " ";     }     std::cout << std::endl;      std::cout << "Sorted array: ";     for (int const& num : sorted_array)     {         std::cout << num << " ";     }     std::cout << std::endl;      return 0; } ``` |

|  |  |
| --- | --- |
| ``` 1 2 3 4 ``` | ``` $ g++ counting_sort.cpp -o counting_sort $ ./counting_sort Array to be sorted: 4 2 2 8 3 3 1 Sorted array: 1 2 2 3 3 4 8 ``` |

## Radix Sort for Integer Numbers

### Radix Sort for Non-Negative Integer Numbers

Radix sort is a non-comparative integer sorting algorithm that sorts numbers by processing individual digits. It works by sorting the input numbers based on each digit, starting from the least significant digit (LSD) to the most significant digit (MSD) or vice versa. Radix sort typically uses counting sort as a subroutine to sort the digits.

For example, consider the following array of integers:

|  |  |
| --- | --- |
| ``` 1 ``` | ``` input_array = [170, 45, 75, 90, 802, 24, 2, 66] ``` |

We can sort the array by processing each digit from the least significant to the most significant.

First, we sort the array based on the least significant digit (LSD) using counting sort:

|  |  |
| --- | --- |
| ``` 1 2 ``` | ``` # After sorting by LSD output_array = [170, 90, 802, 2, 24, 45, 75, 66] ``` |

Note that because counting sort is stable, the relative order of elements with the same digit is preserved. For example, 170 comes before 90 because it appeared first in the original array.

Next, we sort the array based on the next significant digit (the tens place) using counting sort. Those with no tens digit are considered to have a tens digit of 0.

|  |  |
| --- | --- |
| ``` 1 2 ``` | ``` # After sorting by tens place output_array = [802, 2, 24, 45, 66, 170, 75, 90] ``` |

Finally, we sort the array based on the most significant digit (the hundreds place) using counting sort. Similarly, those with no hundreds digit are considered to have a hundreds digit of 0.

|  |  |
| --- | --- |
| ``` 1 2 ``` | ``` # After sorting by hundreds place output_array = [2, 24, 45, 66, 75, 90, 170, 802] ``` |

### Python Implementation

The radix sort algorithm can be implemented using Python as follows. This implementation assumes that all input numbers are non-negative integers.

radix\_sort.py

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 ``` | ``` def counting_sort_for_radix(arr, exp):      n = len(arr)     output = [0] * n     count = [0] * 10  # Since we are dealing with base 10      # Store count of occurrences in count[]     for i in range(n):         index = (arr[i] // exp) % 10         count[index] += 1      # Change count[i] so that it contains the ac...
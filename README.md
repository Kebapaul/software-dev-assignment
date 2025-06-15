# software-dev-assignment
"""
Title: Fermat Near Miss Finder
File: fermat_near_miss.py
External files required: None
External files created: None
Programmers: Alice Smith (asmith@example.edu), Bob Lee (blee@example.edu)
Course: CS101 - Section 2
Date Submitted: 2025-06-15

Description:
This program searches for "near misses" to Fermat’s Last Theorem. 
Given an exponent n (with 2 < n < 12) and an upper bound k (>10) for base integers x and y, 
it iterates over all combinations of x and y from 10 to k. For each pair, it computes x^n + y^n 
and identifies the integer z such that z^n is the closest to x^n + y^n.

It reports the smallest relative miss found, printing out each time a new smallest relative 
miss is discovered, and summarizing the final smallest miss at the end.

Resources Used:
- Python documentation on integer math and power functions
- https://realpython.com/python-int-sequence/#finding-the-closest-integer
"""

# Importing required standard library
import math

def main():
    # Prompt user for exponent n and upper bound k
    print("Fermat Near Miss Finder")
    print("-----------------------")

    # Input validation for exponent n
    while True:
        try:
            n = int(input("Enter exponent n (3 <= n < 12): "))
            if 2 < n < 12:
                break
            else:
                print("Error: n must be an integer between 3 and 11 (inclusive).")
        except ValueError:
            print("Invalid input. Please enter an integer.")

    # Input validation for upper limit k
    while True:
        try:
            k = int(input("Enter maximum value for x and y (must be > 10): "))
            if k > 10:
                break
            else:
                print("Error: k must be greater than 10.")
        except ValueError:
            print("Invalid input. Please enter an integer.")

    smallest_relative_miss = float('inf')  # Tracks smallest relative miss found
    best_x = best_y = best_z = 0           # Store best match
    best_miss = 0                          # Store smallest absolute miss

    # Iterate over all valid x, y pairs
    for x in range(10, k + 1):
        for y in range(10, k + 1):
            lhs = x ** n + y ** n  # Calculate left-hand side: x^n + y^n

            # Find the integer z such that z^n is just below lhs
            z = int(lhs ** (1 / n))

            # Compute both z^n and (z+1)^n to find the closest
            z_power = z ** n
            z1_power = (z + 1) ** n

            # Find the closest to lhs
            miss = min(abs(lhs - z_power), abs(z1_power - lhs))
            relative_miss = miss / lhs

            # If this is the smallest relative miss so far, report it
            if relative_miss < smallest_relative_miss:
                smallest_relative_miss = relative_miss
                best_x, best_y = x, y
                best_z = z if abs(lhs - z_power) < abs(z1_power - lhs) else z + 1
                best_miss = miss

                # Display the new best near miss
                print(f"\nNew smallest relative miss found:")
                print(f"x = {best_x}, y = {best_y}, z = {best_z}, n = {n}")
                print(f"Actual miss = {best_miss}")
                print(f"Relative miss = {relative_miss:.10f}")

    # Final result after all iterations
    print("\n================ Final Result ================")
    print(f"Best near miss for n = {n}, with x and y in range [10, {k}]:")
    print(f"x = {best_x}, y = {best_y}, z = {best_z}")
    print(f"Absolute miss = {best_miss}")
    print(f"Relative miss = {smallest_relative_miss:.10f}")
    input("\nPress Enter to exit the program...")  # Pause before exiting

# Run the main function
if __name__ == "__main__":
    main()

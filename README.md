# software-dev-assignment
"""
Title: Fermat Near Miss Finder
File: fermat_near_miss.py
External files required: None
External files created: None
Programmers: Keba Paul (kebapaul@lewisu.edu)
Course: SU25-CPSC-60500-001
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


# Get the exponent "n" from the user.
# I'm going to loop until we get a valid integer in the desired range.
while True:
    try:
        user_input = input("Enter exponent n (3 <= n < 12): ")
        exponent_n = int(user_input)  # converting input to integer
        if 3 <= exponent_n < 12:
            break
        else:
            print("Error: n must be an integer between 3 and 11 (inclusive).")
    except ValueError:
        print("Invalid input. Please type a legitimate integer.")

# Now, get the maximum value for x and y. We require it to be > 10.
while True:
    try:
        k_str = input("Enter maximum value for x and y (must be > 10): ")
        upper_bound = int(k_str)
        if upper_bound > 10:
            break
        else:
            print("Error: Maximum value must be greater than 10.")
    except ValueError:
        print("Invalid input. Please enter an integer.")

# Initializing tracking variables for the best near-miss we find
min_relative_error = float('inf')  # This will hold the smallest ratio of miss/lhs we see
opt_x = 0  # Best x value found so far
opt_y = 0  # Best y value found so far
opt_z = 0  # Best z value guess
best_absolute_miss = 0  # The smallest absolute miss encountered

# Now, we iterate over all x, y pairs within our permitted range.
for curr_x in range(10, upper_bound + 1):
    for curr_y in range(10, upper_bound + 1):
        # Compute the left-hand side: x^n + y^n. (I could've stored power values but this is fine)
        lhs_value = curr_x ** exponent_n + curr_y ** exponent_n

        # I try to estimate z by taking the nth-root of lhs_value.
        approx_z = int(lhs_value ** (1 / exponent_n))
        
        # Sometimes, the true near integer is one more, so we compute both candidates:
        approx_z_power = approx_z ** exponent_n
        candidate_next_power = (approx_z + 1) ** exponent_n

        # Calculate the absolute difference ("miss") for both candidate z values.
        diff_with_approx = abs(lhs_value - approx_z_power)
        diff_with_next = abs(candidate_next_power - lhs_value)
        
        # Find which candidate gives the smaller miss:
        current_miss = diff_with_approx if diff_with_approx < diff_with_next else diff_with_next

        # For the relative miss, we compare the error to the value of the left-hand side.
        current_relative = current_miss / lhs_value

        # Update our best record if we found a better near miss.
        if current_relative < min_relative_error:
            min_relative_error = current_relative
            
            opt_x = curr_x
            opt_y = curr_y
            # Decide which candidate was closer:
            if diff_with_approx < diff_with_next:
                opt_z = approx_z
            else:
                opt_z = approx_z + 1
                
            best_absolute_miss = current_miss

            # I print a new record each time we update, to let the user follow along.
            print("\nNew smallest relative miss found:")
            print("x =", opt_x, ", y =", opt_y, ", z =", opt_z, ", n =", exponent_n)
            print("Absolute miss =", best_absolute_miss)
            print("Relative miss = {:.10f}".format(min_relative_error))
            # Note: Extra logging can be removed if too verbose later.

# All done; show the final best near-miss.
print("\n================ Final Result ================")
print("For exponent n =", exponent_n, "and with x, y in range [10, {}]:".format(upper_bound))
print("Best combination: x =", opt_x, ", y =", opt_y, ", z =", opt_z)
print("Absolute miss =", best_absolute_miss)
print("Relative miss = {:.10f}".format(min_relative_error))

# Just to keep the console window open (especially useful in Windows environments)
input("\nPress Enter to exit the program...")
# Run the main function
if __name__ == "__main__":
    main()

 Library Books Returns Register  -  README
 Single-file C++17 program: doubly linked list (A1) + templated sorts and
 iterative binary search (A2) + built-in test suite (A3) + timing bench (A4)

 1. BUILDING

 Requires g++ with C++17 support.

   Normal (optimised):
     g++ -std=c++17 -O2 library_register.cpp -o library_register

   Debug / sanitizer build (memory-error checking, used for the A3 evidence):
     g++ -std=c++17 -g -fsanitize=address,undefined -fno-omit-frame-pointer \
         library_register.cpp -o register_asan

   If the LAST DIGIT of your registration number is ODD, the O(n log n) sort
   used by the menu must be quicksort with a random pivot. Build the menu
   program with:
     g++ -std=c++17 -O2 -DUSE_MERGE_SORT=0 library_register.cpp -o library_register
   (Even last digit -> merge sort, which is the default. Both sorts are always
   compiled in and exercised by the test suite regardless of this switch.)

 2. RUNNING

   ./library_register                Interactive menu. On first run it creates a
                                     sample data file 'returns.txt' (12 records),
                                     loads it, and shows the menu. Choose 0 to
                                     save and quit.
   ./library_register test           Runs the A3 test table (test IDs A1-01...
                                     A2-19) and prints PASS/FAIL per row plus a
                                     total. Exit code 0 = all passed.
   ./library_register bench          Runs the A4 timing experiment (insertion,
                                     merge, quicksort on random data for
                                     n = 1000, 2000, ..., 64000, 3 repeats each,
                                     averages and doubling ratios printed as CSV
                                     on stdout - redirect with > timings.csv).
   ./library_register myfile.txt     Menu mode using a different data file.
                                     If the file does not exist a sample file is
                                     created at that path first.

   Sanitizer check (A3 evidence):
     ./register_asan test            Should end with 'TOTAL: 54 passed, 0 failed'
                                     and print nothing on stderr.

 3. DATA FILE FORMAT

   Text file, one record per line:
     id|memberName|bookTitle|dueDate|returnDate
   - id: positive integer, unique (arrival/slip order)
   - dates: strict YYYY-MM-DD, validated against the real calendar
   - text fields must be non-empty and must not contain '|'
   - lines starting with '#' are comments; blank lines are ignored
   - malformed lines are skipped and counted as "skipped" on load
   Example line:
     1001|Achieng Grace|Things Fall Apart|2026-10-20|2026-10-22

 4. INTERACTIVE MENU (quick reference)

    1 Add return (end)    2 Add return (front)   3 Remove by id
    4 Remove first        5 Remove last          6 Find by id
    7 Show forward        8 Show backward        9 Sort
   10 Search by return date (binary search)  11 Save  12 Reload  0 Save & quit

   Option 9 asks for the sort key (return date / member / title / fine desc / id)
   and the algorithm (1 = insertion sort, 2 = O(n log n) sort). Option 10
   requires the register to be sorted by return date - the program sorts a copy
   internally before searching.

 5. FINES

   Fine = 500 UGX per day late; 0 if returned on or before the due date.


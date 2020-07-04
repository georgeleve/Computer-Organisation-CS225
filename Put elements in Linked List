                                # Georgios Gerasimos Leventopoulos csd4152      # 
                                # Assignment 7.3   hy225  Computer Organisation  #
.data
   	newline: .asciz " "
	askUser: .asciz "\nPlease enter an integer: "
	part1: .asciz "Part 1: Adding the elements in the linked list.\n"
	part2: .asciz "Part 2: The elements that are greater than the integer you are going to give in the linked list are:\n"
.text
	# Call main method
	jal main          # Call main method
	addi a7, x0, 10   # Exit the program with code 0
	ecall             # Return 0;
main:   
    addi sp, sp, -4    # Take space in stack for one word.
    sw  ra, 0(sp)      # Store register ra (return address of main method) in stack
    
    # Print string 
    addi a7, x0, 4    # Environment call code for print_string
    la a0, part1       # Pseudo-instruction: address of string
    ecall              # Print string
    
    # Initialize 1st element in linked list.
    jal node_alloc    
    addi s0, a0, 0   # s0 Pointer to the first node.
    addi s1, a0, 0   # s1 Pointer to the last node.
    sw x0, 0(s1)     # list.data = 0 
    sw x0, 4(s1)     # list.nxtPtr = NULL 
    
loopA:
    # Read an integer from the user
    jal read_int                   # a0 = read_int()
    blez a0, print_between_loops   # Stop if integer=a0 <= 0 
    addi s2, a0, 0                 # Keep the adress of a0
    
    # Create a node that holds this number (s2)
    jal node_alloc    # a0 = node_alloc() (Address for new node)
    sw s2, 0(a0)      # list.data = s2
    sw x0, 4(a0)      # list.nxtPtr = 0
    
    # Add node in linked list
    sw a0, 4(s1)      # list.nxtPtr = newNode
    addi s1, a0, 0    # s1 = newNode
    
    j loopA           # Loop
    
print_between_loops:
    addi a7, x0, 4   # Environment call code for print_string
    la a0, part2     # Pseudo-instruction: address of string
    ecall            # Print the string 
  	
loopB:
	# Read numbers from the user, exit if input < 0.
    jal read_int                 # a0 = read_int()
    bltz a0, return_from_main    # If a0 < 0 then return.
    addi s1, a0, 0               # s1 = a0 (Number that the user gave)
    
    # Traverse linked list and print the elements. 
    lw s2, 4(s0)     # s2 = the address of the 1st node.
    jal search_list  # Print values in linked list that are bigger user input. 
    
    j loopB   	     # Loop
    
return_from_main:
    lw ra, 0(sp)      # Load return address from stack
    addi sp, sp, 4    # Return stack pointer in first position. 
    jr ra, 0          # Return

# Traverse linked list starting from address s2
# Print node if list.data > s1(user input)
search_list:
    addi sp, sp, -4     # Make space in the stack for register ra
    sw ra, 0(sp)        # Store ra in stack

search_loop:
    beq s2, x0, return_from_search  # Return if xs2 == 0

    lw t0, 0(s2)            # t0 = list.data
    beq t0, s1, nextNode    # If t0 <= s1 (list.data >= user input) go to nextNode.
    blt t0, s1, nextNode
    jal print_node          # Else print_node.

        nextNode:
        lw s2, 4(s2)         # s2 = nextNode
        j search_loop        # Loop

    return_from_search:
    lw ra, 0(sp)         #S tore ra to stack
    addi sp, sp, 4       # Dealloc
    jr ra, 0             # Return

# Insert a new node in the linked list.
node_alloc:
    addi a7, x0, 9       # Sbrk - Allocate heap memory
    # 8 bytes (2 words) = 4 bytes for data (integer)  AND  4 bytes for nxtPtr (pointer) that points to the next node
    addi a0, x0, 8       # a0 keeps the address of memory that we allocate.
    ecall                # a0 = node
    jr ra, 0             # Jump register, Return to the main method.

# Reads and Returns an integer number.
read_int:
    addi a7, x0, 4      # Environment call code for print_string
    la a0, askUser      # Pseudo-instruction: address of string
    ecall               # Print string

    addi a7, x0, 5      # Environment call code for read_int
    ecall               # Read int
    add a0, a0, x0      # Copy returned int in register a0 (=x10)

    jr ra, 0            # Return integer

# Print the data from node, s2 = node address
print_node:
    lw a0, 0(s2)          # a0 = s2 = list.data

    addi a7, x0, 1       # Environment call code for print_int
    add a0, a0, x0       # Copy argument to a0
    ecall                # Print int

    addi a7, x0, 4       # Environment call code for print_string
    la a0, newline       # Pseudo-instruction: address of string
    ecall

    jr ra, 0              # Return

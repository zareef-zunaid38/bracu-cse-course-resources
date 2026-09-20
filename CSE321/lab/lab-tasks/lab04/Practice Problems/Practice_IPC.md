# Practice Problems on Inter-Process Communication (IPC)

Detailed specifications and sample terminal outputs for Lab 04 IPC problems: banking application using shared memory & pipes, and office login OTP verification using multi-process message queues.
Docx last modified: 20-07-2026.

## Content

### Task 1: Banking Application with Shared Memory and Pipe

#### Scenario & Architecture
- **Shared Memory Structure**:
  ```c
  struct shared {
      char sel[100];
      int b;
  };
  ```
- **Initial Setup**:
  - `home` parent process creates shared memory and a pipe.
  - Prompts user to select an operation:
    1. Type `a` to Add Money
    2. Type `w` to Withdraw Money
    3. Type `c` to Check Balance
  - Stores selection in `sel` and sets initial balance `b = 1000`.
  - Prints selection and passes control to child process `opr`.

- **Child Process `opr` Execution**:
  - Reads selection from shared memory:
    - If `a`: Prompts for amount. If amount > 0, adds to `b` and prints success with updated balance. If amount $\le 0$, prints `"Adding failed, Invalid amount"`.
    - If `w`: Prompts for amount. If amount > 0 and amount $\le b$, subtracts from `b` and prints success with updated balance. If amount $\le 0$ or amount $> b$, prints `"Withdrawal failed, Invalid amount"`.
    - If `c`: Prints current balance `b`.
    - Any other key: Prints `"Invalid selection"`.
  - Writes `"Thank you for using"` into the pipe and terminates.

- **Parent `home` Termination**:
  - Reads `"Thank you for using"` from pipe, prints it, and removes shared memory with `shmctl(..., IPC_RMID, ...)`.

#### Sample Terminal Runs
- **Run 1 (Add Money)**:
  ```text
  Execution Command in Terminal: ./p1
  Provide Your Input From Given Options:
  1. Type a to Add Money
  2. Type w to Withdraw Money
  3. Type c to Check Balance
  a
  Your selection: a

  Enter amount to be added:
  100
  Balance added successfully
  Updated balance after addition:
  1100
  Thank you for using
  ```
- **Run 2 (Invalid Addition)**:
  ```text
  Enter amount to be added: -600 -> Adding failed, Invalid amount -> Thank you for using
  ```
- **Run 3 (Withdraw Money)**:
  ```text
  Enter amount to be withdrawn: 600 -> Balance withdrawn successfully -> Updated balance: 400 -> Thank you for using
  ```
- **Run 4 (Invalid Withdrawal)**:
  ```text
  Enter amount to be withdrawn: -300 -> Withdrawal failed, Invalid amount -> Thank you for using
  ```
- **Run 5 (Check Balance)**:
  ```text
  c -> Your current balance is: 1000 -> Thank you for using
  ```
- **Run 6 (Invalid Option)**:
  ```text
  o -> Invalid selection -> Thank you for using
  ```

---

### Task 2: Office Workspace Login & OTP Verification with Message Queues

#### Scenario & Architecture
- **Message Queue Structure**:
  ```c
  struct msg {
      long int type;
      char txt[6];
  };
  ```
- **Process Communication Flow**:
  1. `log in` (Parent process):
     - Takes workspace name input from user.
     - If not `"cse321"`, prints `"Invalid workspace name"` and aborts.
     - If `"cse321"`, writes workspace name to message queue (type $T_1$), prints confirmation, and spawns `otp generator`.
  2. `otp generator` (Child 1):
     - Reads type $T_1$ message, prints received workspace name.
     - Generates its PID as OTP string, sends to `log in` (type $T_2$), and sends copy to `mail` (type $T_3$).
  3. `mail` (Child of `otp generator`):
     - Reads type $T_3$ message from queue, prints received OTP.
     - Sends OTP to `log in` (type $T_4$) and terminates.
  4. `log in` Resumes:
     - Reads OTP from `otp generator` (type $T_2$) and prints it.
     - Reads OTP from `mail` (type $T_4$) and prints it.
     - Compares both OTPs: prints `"OTP Verified"` if identical, else `"OTP Incorrect"`.
     - Removes message queue via `msgctl(..., IPC_RMID, ...)`.

#### Sample Terminal Output
```text
Execution Command in Terminal: ./p1
Please enter the workspace name:
cse321
Workspace name sent to otp generator from log in: cse321

OTP generator received workspace name from log in: cse321

OTP sent to log in from OTP generator: 13055
OTP sent to mail from OTP generator: 13055
Mail received OTP from OTP generator: 13055
OTP sent to log in from mail: 13055
Log in received OTP from OTP generator: 13055
Log in received OTP from mail: 13055
OTP Verified
```

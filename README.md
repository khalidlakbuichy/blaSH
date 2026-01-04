# blaSH — A Moroccan-Made Shell 🇲🇦

**blaSH** is a 42 Network project inspired by the Unix **Bash** shell.  
It's a simplified yet functional **command-line interpreter** written in **C**.

The goal of blaSH is to provide hands-on experience in **system programming**, exploring how shells work internally — from **parsing commands** to **managing processes** and **handling signals**.

![C](https://img.shields.io/badge/C-00599C?style=for-the-badge&logo=c&logoColor=white)
![Unix](https://img.shields.io/badge/Unix-Shell-lightgrey?style=for-the-badge&logo=gnu-bash&logoColor=white)
![42](https://img.shields.io/badge/42-Network-000000?style=for-the-badge&logo=42&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

---

## ⚙️ How blaSH Works

blaSH operates through a continuous interactive loop that mimics the behavior of a real shell:

1. **Read** – Displays a prompt and reads the user's input line
2. **Parse** – Breaks the input into tokens and builds a structured command representation
3. **Execute** – Runs the command, handling pipes, redirections, and expansions
4. **Repeat** – Returns to the prompt and waits for the next command

This cycle provides an interactive environment for executing system commands just like in Bash.
```
┌─────────────────────────────────────┐
│  blash$ ls -la | grep txt > out.txt │  ← User Input
└─────────────────────────────────────┘
                 │
                 ▼
         [ Read & Tokenize ]
                 │
                 ▼
         [ Parse into AST ]
                 │
                 ▼
         [ Execute Commands ]
                 │
                 ▼
         [ Display Output ]
                 │
                 ▼
         [ Back to Prompt ]
```

---

## 🧩 Core Components

### 🔹 Lexer (Tokenizer)
- Scans the input line character by character
- Produces a sequence of **tokens** (commands, arguments, operators, etc.)
- Handles quoting and escaping

### 🔹 Parser
- Converts the token stream into an **Abstract Syntax Tree (AST)**
- Defines command relationships and ensures valid syntax
- Validates command structure before execution

### 🔹 Executor
- Traverses the AST and executes each command
- Handles **built-ins**, **external programs**, **pipes**, **redirections**, and **expansions**
- Manages process creation and inter-process communication

---

## 🚀 Features

| Feature | Description |
|:--------|:------------|
| 🧠 **Command Execution** | Supports both built-in and external commands |
| 🔀 **Pipes (`\|`)** | Connects multiple commands for data flow |
| 📤 **Redirections (`>`, `<`, `>>`)** | Redirects input/output streams |
| 💲 **Variable Expansion (`$VAR`)** | Expands environment variables |
| 🗂️ **Wildcard Expansion (`*`, `?`)** | Matches files and directories dynamically |
| 🧩 **Built-ins** | Includes core commands like `cd`, `echo`, `export`, `env`, `unset`, `exit` |
| ⚡ **Signal Handling** | Gracefully manages `SIGINT` (Ctrl+C) and `SIGQUIT` (Ctrl+\\) |
| 🎨 **Custom Prompt** | Displays current directory and user information |
| 📝 **Command History** | Tracks previous commands (using readline) |

---

## 🛠️ Installation & Usage

### Prerequisites
- **GCC** or **Clang** compiler
- **Make**
- **readline** library (for history and line editing)

### Build Instructions
```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/blash.git
cd blash

# Compile the project
make

# Run blaSH
./blash
```

### Clean Build Artifacts
```bash
make clean     # Remove object files
make fclean    # Remove object files and executable
make re        # Rebuild from scratch
```

---

## 💻 Usage Examples

### Basic Commands
```bash
blash$ ls -la
blash$ echo "Hello from blaSH!"
blash$ pwd
```

### Pipes
```bash
blash$ ls | grep .c | wc -l
blash$ cat file.txt | sort | uniq
```

### Redirections
```bash
blash$ echo "test" > output.txt
blash$ cat < input.txt
blash$ ls >> log.txt
```

### Environment Variables
```bash
blash$ export MY_VAR="Hello World"
blash$ echo $MY_VAR
blash$ env | grep MY_VAR
```

### Wildcards
```bash
blash$ ls *.c
blash$ rm test?.txt
```

### Built-in Commands
```bash
blash$ cd /path/to/directory
blash$ export PATH=$PATH:/new/path
blash$ unset MY_VAR
blash$ exit
```

---

## 🧠 Learning Objectives

By building **blaSH**, you will:

- ✅ Understand **process creation** and **inter-process communication**
- ✅ Learn how **shells parse and interpret** user input
- ✅ Implement **system calls** like `fork()`, `execve()`, `pipe()`, and `dup2()`
- ✅ Gain deeper insight into **Unix system behavior** and **signal management**
- ✅ Master **memory management** and **error handling** in C
- ✅ Build a **lexer** and **parser** from scratch

---

## 🏗️ Architecture Overview
```
┌─────────────────────────────────────────────┐
│                   Prompt                    │
│              (readline input)               │
└─────────────────┬───────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────┐
│                   Lexer                     │
│         (Tokenize input string)             │
│    TOKEN_WORD, TOKEN_PIPE, TOKEN_REDIR      │
└─────────────────┬───────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────┐
│                   Parser                    │
│            (Build AST structure)            │
│   Simple Command → Pipeline → Redirections  │
└─────────────────┬───────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────┐
│                  Expander                   │
│      (Variable & Wildcard expansion)        │
│         $VAR, $?, *.c, test?.txt            │
└─────────────────┬───────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────┐
│                  Executor                   │
│     fork() → execve() → wait() → pipe()     │
│         Built-ins vs External cmds          │
└─────────────────┬───────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────┐
│              Command Output(s)              │
│         (stdout/stderr/files)               │
└─────────────────────────────────────────────┘
```

---

## 📂 Project Structure
```
blash/
├── src/
│   ├── main.c              # Entry point and REPL loop
│   ├── lexer/
│   │   ├── tokenizer.c     # Token generation
│   │   └── token_utils.c   # Token manipulation
│   ├── parser/
│   │   ├── parser.c        # AST construction
│   │   └── syntax.c        # Syntax validation
│   ├── expander/
│   │   ├── expand_vars.c   # Variable expansion
│   │   └── expand_wild.c   # Wildcard expansion
│   ├── executor/
│   │   ├── exec_cmd.c      # Command execution
│   │   ├── exec_pipe.c     # Pipeline handling
│   │   ├── exec_redir.c    # Redirection handling
│   │   └── builtins/       # Built-in commands
│   ├── signals/
│   │   └── signal_handler.c # Signal management
│   └── utils/
│       ├── env.c           # Environment variables
│       └── error.c         # Error handling
├── includes/
│   └── blash.h             # Main header file
├── Makefile
└── README.md
```

---

## 🧪 Testing

### Manual Testing
```bash
# Test pipes
blash$ echo "test" | cat | cat | wc -l

# Test redirections
blash$ echo "hello" > file.txt && cat < file.txt

# Test exit codes
blash$ ls non_existent_file
blash$ echo $?

# Test signal handling
blash$ sleep 100
^C  # Should interrupt gracefully
```

### Automated Testing (Optional)
Create a test script:
```bash
#!/bin/bash
./blash < test_input.txt > test_output.txt
diff test_output.txt expected_output.txt
```

---

## 🐛 Known Limitations

- No support for logical operators (`&&`, `||`)
- No support for command substitution (`` `cmd` `` or `$(cmd)`)
- No support for job control (`bg`, `fg`, `jobs`)
- Limited heredoc support (`<<`)
- No brace expansion (`{a,b,c}`)

These are intentional simplifications for the scope of the 42 project.

---

## 🤝 Contributing

Contributions are welcome! If you'd like to improve blaSH:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📚 Resources

- [GNU Bash Manual](https://www.gnu.org/software/bash/manual/)
- [Advanced Programming in the UNIX Environment](https://www.apuebook.com/)
- [The Linux Programming Interface](https://man7.org/tlpi/)
- [42 Cursus - Minishell Subject](https://github.com/42School/minishell)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **42 Network** for the project subject and learning methodology
- The **Unix/Linux** community for shell design inspiration
- All contributors and testers who helped improve blaSH

---

<div align="center">

**Built with 🧠 by [Khalid](https://github.com/YOUR_USERNAME) at 42 Network**

⭐ Star this repo if you find it useful!

Made in Morocco 🇲🇦

</div>

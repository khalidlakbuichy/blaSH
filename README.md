# blaSH — A Moroccan-Made Shell 🇲🇦

**blaSH** is a 42 Network project inspired by the Unix **Bash** shell.  
It’s a simplified yet functional **command-line interpreter** written in **C*.

The goal of blaSH is to provide hands-on experience in **system programming**, exploring how shells work internally — from **parsing commands** to **managing processes** and **handling signals**.

---

## ⚙️ How blaSH Works

blaSH operates through a continuous interactive loop that mimics the behavior of a real shell:

1. **Read** – Displays a prompt and reads the user’s input line.  
2. **Parse** – Breaks the input into tokens and builds a structured command representation.  
3. **Execute** – Runs the command, handling pipes, redirections, and expansions.  
4. **Repeat** – Returns to the prompt and waits for the next command.

This cycle provides an interactive environment for executing system commands just like in Bash.

---

## 🧩 Core Components

### 🔹 Lexer (Tokenizer)
- Scans the input line character by character.  
- Produces a sequence of **tokens** (commands, arguments, operators, etc.).  

### 🔹 Parser
- Converts the token stream into an **Abstract Syntax Tree (AST)**.  
- Defines command relationships and ensures valid syntax.  

### 🔹 Executor
- Traverses the AST and executes each command.  
- Handles **built-ins**, **external programs**, **pipes**, **redirections**, and **expansions**.  

---

## 🚀 Features

| Feature | Description |
|:--|:--|
| 🧠 **Command Execution** | Supports both built-in and external commands. |
| 🔀 **Pipes (`|`)** | Connects multiple commands for data flow. |
| 📤 **Redirections (`>`, `<`, `>>`)** | Redirects input/output streams. |
| 💲 **Variable Expansion (`$VAR`)** | Expands environment variables. |
| 🗂️ **Wildcard Expansion (`*`, `?`)** | Matches files and directories dynamically. |
| 🧩 **Built-ins** | Includes core commands like `cd`, `echo`, and `export`. |
| ⚡ **Signal Handling** | Gracefully manages `SIGINT` (Ctrl+C) and `SIGQUIT`. |

---

## 🧠 Learning Objectives

By building **blaSH**, you will:
- Understand **process creation** and **inter-process communication**.
- Learn how **shells parse and interpret** user input.
- Implement **system calls** like `fork()`, `execve()`, and `pipe()`.
- Gain deeper insight into **Unix system behavior** and **signal management**.

---

## 🏗️ Architecture Overview
+-------------------+
| Prompt |
+-------------------+
        |
        v
+-------------------+
| Lexer |
+-------------------+
        |
        v
+-------------------+
| Parser |
| (Builds AST) |
+-------------------+
        |
        v
+-------------------+
| Executor |
| (Commands, Pipes) |
+-------------------+
        |
        v
+-------------------+
| Command Output(s) |
+-------------------+

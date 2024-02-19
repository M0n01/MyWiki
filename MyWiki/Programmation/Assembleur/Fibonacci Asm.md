
```nasm
global  _start
extern printf, scanf

section .data
        message db "Please input max Fn:", 0x0a
        outFormat db "%d", 0x0a, 0x00
        inFormat db "%d", 0x00

section .bss
        userInput resb 1

section .text
_start:
        call printMessage  ; print message intro
        call getInput      ; demande valeur a user
        call initFib       ; init les valeurs pour Fib
        call loopFib       ; lance le calcule de la Fib
        call Exit          ; exit le programme

printMessage:
        mov rax, 1       ; syscall 1 pour write
        mov rdi, 1       ; fd 1 pour stdout
        mov rsi, message ; pointer to message
        mov rdx, 20      ; print longueur de 20 octets
        syscall          ; appelle syscall write
        ret

initFib:
        xor rax, rax    ; initialise rax = 0
        xor rbx, rbx    ; initialise rbx = 0
        inc rbx         ; incremente rbx de 1
        ret

printFib:
        push rax            ; push les registres dans la stack
        push rbx
        mov rdi, outFormat  ; set 1er argument (print format)
        mov rsi, rbx        ; set 2eme argument (nombre Fib)
        call printf         ; printf(outFormat, rbx)
        pop rbx             ; restaure le registre depuis la stack
        pop rax
        ret

loopFib:
        call printFib           ; print current Fib number
        add rax, rbx            ; rax + rbx
        xchg rax, rbx           ; on echange leur valeurs
        cmp rbx, [userInput]    ; rbx - 10
        js loopFib              ; jump si resultat < 0
        ret

getInput:
        sub rsp, 8
        mov rdi, inFormat    ; set 1st parameter (inFormat)
        mov rsi, userInput   ; set 2nd parameter (userInput)
        call scanf           ; scanf (inFormat, userInput)
        add rsp, 8
        ret

Exit:
        mov rax, 60
        mov rdi, 0
        syscall

```


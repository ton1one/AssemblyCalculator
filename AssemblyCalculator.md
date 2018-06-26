# AssemblyCalculator

ORG 100h              ;Iniciando o programa

include 'emu8086.inc' ;Importando funcoes inclusas da biblioteca do emu8086 
DEFINE_SCAN_NUM       ;Funcao que permite que apenas numeros sejam armazenados no determinado registrador
DEFINE_PRINT_STRING   ;Funcao que permite imprimir cadeias de caracteres em determinadas posicoes
DEFINE_PRINT_NUM      ;Funcao que imprime por padrao um determinado numero no registrador AX
DEFINE_PRINT_NUM_UNS  ;Funcao que imprime por padrao um numero nao determinado no registrador AX

JMP inicio            ;Realiza um salto para a classe "inicio"
         
;CÓDIGO COPIADO DE ANTÔNIO RABELLO, TODOS OS DIREITOS RESEVADOS

;Definindo variaveis de texto no registrador DB, sendo que cada cadeia de texto eh mostrada linha por linha

texto DB 13,10, 13, 10, 'Por favor, digite o primeiro numero: $'
texto2 DB 13,10,13,10,'Por favor, digite o segundo numero: $'
texto3 DB 13,10,13,10, 'Soma = $' 
texto4 DB 13,10,'Subtracao = $' 
texto5 DB 13,10,'Multiplicacao = $'       
texto6 DB 13,10,'Divisao = $'              
texto7 DB 13,10, 13, 10, 'Por favor digite o operador + - * / ou = para FIM: $'
texto_erro DB 13,10, 13, 10, 'INSIRA UM OPERADOR VALIDO $'

numero1 DW ?        ;Definindo variavel que inicializa com valor nulo no registrador DW
numero2 DW ?        ;Definindo variavel que inicializa com valor nulo no registrador DW

inicio: 

MOV AH,09           ;Imprimindo um numero na tela
LEA DX,texto        ;Carrega a primeira cadeia de texto no registrador DX
INT 21h             ;Chama a interrupcao 21h 

CALL SCAN_NUM       ;Chama a funcao SCAN_NUM, que recebe um numero do teclado e armazena no registrador AX
MOV numero1,CX      ;Move o numero digitado para a variavel 

MOV AH,09           ;Imprimindo um numero na tela
LEA DX,texto2       ;Carrega a segunda cadeia de texto no registrador DX
INT 21h             ;Chama a interrupcao 21h  

CALL SCAN_NUM       ;Chama a funcao SCAN_NUM, que recebe um numero do teclado e armazena no registrador AX
MOV numero2,CX      ;Move o numero digitado para a variavel 

MOV AH,09           ;Imprimindo um numero na tela
LEA DX,texto7       ;Carrega a setima cadeia de texto no registrador DX
INT 21h             ;Chama a interrupcao 21h 

MOV AH, 1           ;Move o valor lido para o registrador AH
INT 21h             ;Chama a interrupcao 21h 

caso1:
CMP AL, '+'         ;Compara o valor inserido pelo usuario com o o operador +
JE soma             ;Caso seja igual a +, pula para a funcao soma
JNE caso2           ;Caso seja diferente, pula para a proxima verificacao

caso2:
CMP AL, '-'         ;Compara o valor inserido pelo usuario com o operador -
JE subtracao        ;Caso seja igual a -, pula para a funcao subtracao
JNE caso3           ;Caso seja diferente, pula para a proxima verificacao

caso3:
CMP AL, '*'         ;Compara o valor inserido pelo usuario com o operador *
JE multiplicacao    ;Caso seja igual a *, pula para a funcao multiplicacao
JNE caso4           ;Caso seja diferente, pula para a proxima verificacao

caso4:
CMP AL, '/'         ;Compara o valor inserido pelo usuario com o operador /
JE divisao          ;Caso seja igual a /, pula para a funcao divisao
JNE caso5           ;Caso seja diferente, pula para a proxima verificacao

caso5:
CMP AL, '='         ;Compara o valor inserido pelo usuario com =
JE fim              ;Caso seja igual a =, pula para o fim do programa
JNE erro            ;Caso seja diferente, significa que nao se enquadrou e nenhum dos operadores validos, entao, pula para a mensagem de erro

subtracao:
MOV AH,09           ;Imprimindo um numero na tela
LEA DX,texto4       ;Carrega a quarta cadeia de texto no registrador DX
INT 21h             ;Chama a interrupcao 21h 
MOV AX,numero1      ;Move o primeiro numero digitado para o registrador AX
SUB AX,numero2      ;Subtrai o segundo numero com o primeiro e armazena 
CALL PRINT_NUM      ;Mostra o resultado na tela
JMP inicio          ;Volta para o inicio do programa
 
soma:
MOV AH,09           ;Imprimindo um numero na tela
LEA DX,texto3       ;Carrega a terceira cadeia de texto no registrador DX
INT 21h             ;Chama a interrupcao 21h 
MOV AX,numero1      ;Move o primeiro numero digitado para o registrador AX
ADD AX,numero2      ;Soma o segundo numero com o primeiro e armazena
CALL PRINT_NUM      ;Mostra o resultado na tela
JMP inicio          ;Volta para o inicio do programa

multiplicacao:              
MOV AH,09           ;Imprimindo um numero na tela
LEA DX,texto5       ;Carrega a quinta cadeia de texto no registrador DX
INT 21h             ;Chama a interrupcao 21h 
MOV AX,numero1      ;Move o primeiro numero digitado para o registrador AX 
MOV BX,numero2      ;Move o segundo numero digitado para o registrador BX
MUL BX              ;Multiplica os valores de AX com BX
CALL PRINT_NUM      ;Mostra o resultado na tela
JMP inicio          ;Volta para o inicio do programa
     
divisao:     
MOV AH,09           ;Imprimindo um numero na tela
LEA DX,texto6       ;Carrega a sexta cadeia de texto no registrador DX
INT 21h 
XOR DX,DX           ;Deixando o registrador DX com valor 0, para nao extrapolar a divisao, pois ele armazena o modulo da divisao
MOV AX,numero1      ;Move o primeiro numero digitado para o registrador AX 
MOV BX,numero2      ;Move o segundo numero digitado para o registrador BX
DIV BX              ;Divide os valores de AX com BX  
CALL PRINT_NUM      ;Mostra o resultado na tela                                 
JMP inicio          ;Volta para o inicio do programa

erro:                                      
MOV AH,09           ;Imprimindo um numero na tela
LEA DX,texto_erro   ;Carrega o texto de erro no registrador DX
INT 21h             ;Chama a interrupcao 21h
JMP inicio          ;Volta para o inicio do programa



fim: 
HLT ;Termina a execucao do programa

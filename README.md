
# Verificador de CPF

## Etapas:
###       - Primeira: Verificar se o CPF  informado contém todos os dígidos nem são repetidos
###       - Segunda: Confirmar se o primeiro dígito do verificador está correto
###       - Terceira: Confirmar se o segundo dígito do verificador está correto
###       - Quarta: Validar o CPF
###       - Quinta: Informar o estado emissor do CPF 


## pseudocódigo:
https://www.geradorcpf.com/algoritmo_do_cpf.htm





# Primeira etapa
## Verificar se o CPF  informado contém todos os dígidos nem são repetidos


```python
def str_to_int(string) :
    
    vector = [0,0,0,0,0,0,0,0,0,0,0]
    
    for i in range( len(string) ):  
        vector[i] = int (string[i])
    
    return vector
```


```python
### Utilizar no futuro a estrutura |thrown exception| 

def checkerCPF(cpf):
    
    checked = False
    
    # verificar se a string possui tamanho igual a 11
    if (len(cpf) == 11) :
        
        # verificar se cpf não contém todos os valores idênticos
        for i in range(11):  
            if ( cpf[1] != cpf[i] ):
                checked = True   
    
    return checked
```

# Kernel 
## Segunda e a Terceira etapa


```python
import math

def checked_digit(cpf,digito):
    
    sum = 0
    
    for i in range( 8 + digito):
        
        ######################## Primeiro Dígito
        # Distribua os 9 primeiros dígitos em um quadro (Ex.: cpf[i]);
        # Atribuindo pesos respectivos a eles: seguindo a fórmula: 8 + 1 - i;
        # Multiplique os valores de cada coluna (Ex.: sum += (cpf[i] * (9 + 1 - i)))
        # Esquematizando: 
            #             01	01	01	04	04	04	07	07	07
            #          x  10	09	08	07	06	05	04	03	02
            #             ------------------------------------
            #             10	09	08	28	24	20	28	21	14
            
        ######################## Segundo Dígito
        # Distribua os 10 primeiros dígitos em um quadro (Ex.: cpf[i]);
        # Atribuindo pesos respectivos a eles: seguindo a fórmula: 8 + 2 - i;
        # Multiplique os valores de cada coluna (Ex.: sum += (cpf[i] * (9 + 2- i)))
        # Esquematizando: 
            #             01	01	01	04	04	04	07	07	07  03
            #          x  11	10	09	08	07	06	05	04	03	02
            #             -----------------------------------------
            #             11	10	09	32	28	24	35	28	21	06
    

        sum += (cpf[i] * (9 + digito - i))
    
    # O somatório obtido será divido por 11 (Ex.: (sum/11))
    # Considere como quociente apenas o valor inteiro, neste caso o quociente é: 14;
    # O resto da divisão será responsável pelo cálculo do primeiro dígito verificador, 
    # neste caso o resto é: 727272727272727.
    
    sum /= 11
    
    # Se, o resto da divisão seja menor que 2, o primeiro dígito verificador se torna 0;
    # Se não, subtrai-se do quociente o valor do resto da divisão aproximado para cima vezes 10 
    # Ex.: quociente - (ceil(resto_divisão))*10
    
    quociente = int(sum) 
    resto_divisao = math.ceil((sum - quociente) * 10)
    
#     print('quociente =', quociente)
#     print('resto da divisao =', resto_divisao)
    
    if ( resto_divisao < 2): 
        
        return (0)
    else:
        return (11 - resto_divisao)
```

# Quarta etapa: Main


```python
def verificador(cpf):
    
    checker = False
    
    if checkerCPF(cpf) :
        
        if ( (cpf[9] == checked_digit(cpf,1)) and (cpf[10] == checked_digit(cpf,2))) :
            checker = True
            
    return checker
```

# Quinta etapa
## Informar o estado emissor do CPF, tal informação está disponível no nono dígito do CPF.
### Exemplo de CPF: 000.000.002-00, no qual o dígito 2 apresenta o código do estado emissor.
#### A tabela dos códidos estados emissores é a seguinte:

##### 0 - Rio Grande do Sul;
##### 1 - Distrito Federal, Goiás, Mato Grosso do Sul e Tocantins;
##### 2 - Pará, Amazonas, Acre, Amapá, Rondônia e Roraima;
##### 3 - Ceará, Maranhão e Piauí;
##### 4 - Pernambuco, Rio Grande do Norte, Paraíba e Alagoas;
##### 5 - Bahia e Sergipe;
##### 6 - Minas Gerais;
##### 7 - Rio de Janeiro e Espírito Santo;
##### 8 - São Paulo;
##### 9 - Paraná e Santa Catarina.


```python
def estado_emissor(cpf):
    
    if cpf[8] == 0:
        emissor = 'Rio Grande do Sul'

    elif cpf[8] == 1:
        emissor = 'Distrito Federal, Goiás, Mato Grosso do Sul ou Tocantins;'

    elif cpf[8] == 2:
        emissor = 'Pará, Amazonas, Acre, Amapá, Rondônia ou Roraima'

    elif cpf[8] == 3:
        emissor = 'Ceará, Maranhão ou Piauí'

    elif cpf[8] == 4:
        emissor = 'Pernambuco, Rio Grande do Norte, Paraíba ou Alagoas'

    elif cpf[8] == 5:
        emissor = 'Bahia ou Sergipe'

    elif cpf[8] == 6:
        emissor = 'Minas Gerais'

    elif cpf[8] == 7:
        emissor = 'Rio de Janeiro ou Espírito Santo'

    elif cpf[8] == 8:
        emissor = 'São Paulo'

    elif cpf[8] == 9:
        emissor = 'Paraná ou Santa Catarina'

    else: 
        emissor = 'Error: Emissor do CPF desconhecido'
    
    return emissor
```


```python
def verificador_cpf(cpf):
    
    message = 'CPF inválido'
    
    cpf = (str_to_int(cpf))
    
    if ( verificador(cpf) ):       
        message = ('CPF válido, tendo o seguinte estado emissor: ' + estado_emissor(cpf))
    
    return message
```

# Main


```python
cpf = (input('Digite o CPF que deseja verificar:'))

print(verificador_cpf(cpf))
```

    Digite o CPF que deseja verificar: 12312312312


    CPF inválido


# Dicionários e conjuntos

Dicionário (*dict*) e conjunto (*set*) e são duas das mais importantes estruturas de dados em Python. Vamos começar pelos dicionários.

## Dicionários

Dicionários em especial são tão poderosos e flexíveis que são usados internamente para fazer funcionar a linguagem Python, e por isso existem piadas sobre linguagens de programação que são mais ou menos assim:

> E se tudo fosse uma lista? LISP
> 
> E se tudo fosse uma pilha (*stack*)? Forth
>
> E se tudo fosse um ponteiro? C
> 
> E se tudo fosse um dicionário? Python!

Ou ainda este meme aqui:

![](https://pbs.twimg.com/media/EelzOpCX0AAIeYV?format=png&name=small)

### Uma metáfora para dicionários

Imagine um dicionário como uma tabela de duas colunas que permite que busquemos um item, que chamamos de **chave**, o procurando na coluna da esquerda. Se o encontrarmos podemos consultar na coluna da direita um **valor** correspondente. Um dicionário é feito de pares chave-valor. 

Como regra simplificada, podemos usar como chaves objetos *imutáveis*, como números, texto (*strings*) ou tuplas cujos elementos internos também sejam imutáveis. Não podemos usar listas, uma vez que são *mutáveis*. Já os valores podem ser qualquer tipo de objeto/valor de Python, incluido listas e até mesmo outros dicionários!

> Uma explicação mais detalhadas sobre as limitações técnicas dos tipos que podemos usar nos dicionários não cabe neste texto introdutório, mas a sua curiosidade pode fazer você querer ler mais sobre eles em [Estruturas de dados (na documentação do Python)](https://docs.python.org/pt-br/3/tutorial/datastructures.html#dictionaries).

Vejamos um exemplo prático em que um dicionário serve para guardar uma paleta de cores nomeadas, os nomes das cores vão ser as chaves, e as cores produzidas pela função `color()`do Processing vão ser os valores (que podem no final das contas serem usados nas funções `fill()`, `stroke()` e `background()`, por exemplo).

| chaves (*keys*) | valores (*values*) |
| --------------- | ------------------ |
| "branco"        | color(255)         |
| "preto"         | color(0)           |
| "azul"          | color(0, 0, 200)   |
| "amarelo"       | color(220, 220, 0) |
| "vermelho"      | color(200, 0, 0)   |

Em Python podemos definir um dicionário diretamente no código com a sintaxe `{chave: valor,}`.

```python
cores = {
    "branco": color(255),
    "preto": color(0),
    "azul": color(0, 0, 200), 
    "amarelo": color(220, 220, 0),
    "vermelho": color(200, 0, 0),
    }
```

Para consultar o valor atribuido a uma chave, acrescentar uma nova chave, ou modificar o valor dela usamos colchetes `[chave]`.

```python
cor_fundo = cores['azul']  # obtém a cor atribuida à chave 'azul'
background(cor_fundo)

cores['verde']  = color(0, 200, 0)  # acrescentar 'verde' ao dicionário
cores['amarelo'] = color(255, 255, 0) # modifica valor de 'amarelo'
```

No caso da consulta, se não houver a chave no dicionário, teremos um erro! Se não temos certeza da existência da chave podemos usar uma segunda forma de consulta com `.get()`.

```python
cinza = cores['cinza']  # KeyError!

laranja = cores.get('laranja')  # Caso não haja 'laranja' obtemos `None`
if laranja:          # None é considerado 'False' e neste caso
    fill(laranja)    # em um primeiro momento fill() não executa

# podemos também propor um resultado padrão quando a chave não está lá
roxo = cores.get('roxo', color(200))  # se não houver 'roxo' cinza claro
fill(roxo)  # enquanto não houver 'roxo' no dicionário teremos color(200)
```

### Um exemplo com tuplas como chaves: um tabuleiro

```python
# Exemplo de diconário com uma tupla como chave - batalha naval

tam_tabuleiro = 15
tam_casa = 35
meia_casa = tam_casa / 2
borda = 36
tabuleiro_a = {
    (1, 1): "C",
    (2, 1): "C",
    (3, 1): "C",
    (4, 1): "C",
    (6, 6): "S",
    (10, 6): "H",
    (11, 7): "H",
    (12, 6): "H",
   }

cores = {
    "C": color(100, 0, 0),
    "S": color(0, 0, 100),
    "H": color(0, 100, 0),
    }

def setup():
    size(600, 600)
    textAlign(CENTER, CENTER)
    
def draw():
    background(200)
    for i in range(tam_tabuleiro):
        for j in range(tam_tabuleiro):
            c = tabuleiro_a.get((i, j))
            if not c:
                fill(255)
            else:
                fill(cores[c])
            square(i * tam_casa + borda, 
                   j * tam_casa + borda, 
                   tam_casa)
            if c:
                fill(255)
                text(c,
                     i * tam_casa + borda + meia_casa, 
                     j * tam_casa + borda + meia_casa) 
    
    fill(0)
    for n in range(tam_tabuleiro):
        pos =  n * tam_casa + borda + meia_casa 
        text(n, meia_casa, pos)
        text(n, pos, height - meia_casa)
```

![imagem do tabuleiro aqui](assets/batalha-naval.png)


### Exemplo de diconário com dicionários dentro

Não é incomum termos como valores de um dicionário, outros dicionários. O formato de intercâmbio de infomações JSON (lê-se *djeizon*, vem de *JavaScript Object Notation*), é praticamente uma porção de dicionários aninhados.

```python
# Fonte IBGE 2020

estados = {'MG': {'capital':'Belo Horizonte', 'pop': 21292666},
           'AC': {'capital':'Rio Branco', 'pop': 894470},
           'RJ': {'capital':'Rio de Janeiro', 'pop': 17366189},
           'BH': {'capital':'Salvador', 'pop': 14930634},
           'PR': {'capital':'Curitiba', 'pop': 11516840},
           'AC': {'capital':'Rio Branco', 'pop': 894470},
           'RS': {'capital':'Porto Alegre', 'pop': 11422973},
           'PE': {'capital':'Recife', 'pop': 9616621},
           'CE': {'capital':'Fortaleza', 'pop': 9187103},
           'PA': {'capital':u'Belém', 'pop': 8690745},
           'SC': {'capital':'Joinville', 'pop': 7252502},
           'MA': {'capital':u'São Luís', 'pop': 7114598},
           'GO': {'capital':u'Goiânia', 'pop': 7113540},
           'AM': {'capital':'Manaus', 'pop': 4207714},
           'ES': {'capital':u'Vitória', 'pop': 4064052},
           'PB': {'capital':u'João Pessoa', 'pop': 4039277},
           'RN': {'capital':'Natal', 'pop': 3534165},
           'MT': {'capital':u'Cuiabá', 'pop': 3526220},
           'AL': {'capital':u'Maceió', 'pop': 3351543},
           'TO': {'capital':u'Palmas', 'pop': 1607363},
           'MS': {'capital': u'Campo Grande', 'pop': 2839188},
           }

estados['SP'] = {'capital':u'São Paulo', 'pop':46289333}  # Acrescenta estado, a chave é a sigla, o valor um outro dicionário
   
```

### A questão da ordem dos elementos

Vale notar que até pouco tempo atrás os dicionários comuns em Python não guardavam ou garantiam a ordem das chaves. Em Python 3 atual isso mudou, e em Python 2 é possível recorrer a `OrderedDict` se você precisar manter registro da ordem em que as chaves foram criadas.

## Conjuntos

Conjuntos (sets) são estruturas para guardar coleções de itens sem se preocupar com a ordem (por isso não nos referimos a eles como sequências como as tuplas e listas). 

- Converter uma coleção em conjunto garante que não temos repetição de itens (mas perderemos a ordem se originalmente tínhamos uma coleção ordenada, uma sequência)
- São super eficientes para a consulta de existência ou não de um item  (temos X neste conjunto?) assim como operações de subtração, união e intersecção de conjuntos. 

### Eliminando repetições com conjuntos

Uma das formas mais simples de se eliminar duplicações, items repetidos, em uma lista, é transformá-la em um conjunto e depois de volta em uma lista (perde-se a ordem dos elementos).

```python
frutas = ['banana', 'uva', 'uva', 'banana', 'kiwi', 'jaca', 'uva']

frutas_sem_repetir = list(set(frutas)) # resultado: ['banana', 'uva', 'kiwi', 'jaca']
```

### Operações com conjuntos e seus métodos

```python
conjunto_a = {"mar", "vento",}
conjunto_b = {"fogo", "vento"}

uniao = conjunto_a | conjunto_b                 # {"mar", "vento", "fogo"} 
interseccao = conjunto_a & conjunto_b           # {"vento"}
diferenca_simetrica = conjunto_a ^ conjunto_b   # {"mar", "fogo"} 
diferenca_a_menos_b = conjunto_a - conjunto_b   # {"mar"}
diferenca_b_menos_a = conjunto_b - conjunto_a   # {"fogo"}
```

#### Um exemplo visual, interativo

![conjuntos](assets/conjuntos.png)

```python
from __future__ import unicode_literals

def setup():
    global conjuntos
    size(900, 900)
    set1, set2 = set(), set()
    c1x, c1y, c2x, c2y = 300, 300, 600, 300
    textFont(createFont('Source Code Pro Bold', 24))
    for i in range(2000):
        x = random(width)
        y = random(height)
        if dist(x, y, c1x, c1y) < 270:
            set1.add((x, y))
        if dist(x, y, c2x, c2y) < 270:
            set2.add((x, y))
    conjuntos = (   # Atenção, `conjuntos` é uma tupla de dicionários! Contém conjuntos na chave 'set'
        {'set' : set1 - set2,'label' : 'set1 - set2 (diferença)',
        'visible': True, 'diameter': 20, 'color' : color(200, 0, 0)},                  
        {'set' : set2 - set1,'label' : 'set2 - set1 (diferença)',
        'visible': True, 'diameter': 20, 'color' : color(0, 0, 200)},                           
        {'set' : set1 | set2,'label' : 'set1 | set2 (união)',
        'visible': True, 'diameter': 18, 'color' : color(200, 200, 200)},           
        {'set' : set1, 'label' : 'set1',
        'visible': True, 'diameter': 14, 'color' : color(0, 200, 200)},
        {'set' : set2, 'label' : 'set2',
        'visible': True, 'diameter': 10, 'color' : color(200, 200, 0)},
        {'set' : set1 & set2, 'label' : 'set1 & set2 (intersecção)',
        'visible': True, 'diameter': 6, 'color' : color(0, 200, 0)},
        {'set' : set1 ^ set2, 'label' : 'set1 ^ set2 (diferença simétrica)',
        'visible': True, 'diameter': 6, 'color' : color(255, 0, 255)},
    )             
    
def draw():
    background(100)
    noStroke()
    textSize(24)
    for i, c in enumerate(conjuntos):
        fill(c['color'], 100 + 155 * c['visible'])
        text(c['label'], 30, 650 + i * 30)
        if c['visible']:
            for x, y in c['set']:
                circle(x, y, c['diameter'])        
    
def mouseClicked():
    for i, c in enumerate(conjuntos):
        y = 650 + i * 30 - 20
        if y < mouseY < y + 30 and 30 < mouseX < 30 + textWidth(c['label']):
            c['visible'] = not c['visible']
```

Execute o código acima para poder com o mouse ligar e desligar cada um dos conjuntos calculados pelas operações demonstradas (clicando na legenda).

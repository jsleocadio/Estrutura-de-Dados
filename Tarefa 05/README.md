# Tarefa 05

### Aluno: Jefferson dos Santos Leocadio

### Tarefa:

## Vocês deverão adaptar (em relação as classes criadas) a solução do "Guided Project: Building Fast Queries on a CSV" (presente no curso "Algorithm Complexity") para o dataset https://www.kaggle.com/datasets/pavellexyr/the-reddit-climate-change-dataset.

A classe original:

```
class Inventory:
    def __init__(self, csv_filename):
        self.filename = csv_filename
        self.header = []
        self.rows = []
        with open(self.filename) as f:
            self.rows = list(csv.reader(f))
            self.header = self.rows[0]
            del self.rows[0]
            
        for row in self.rows:
            row[-1] = int(row[-1])
            
        self.id_to_row = {}
        for row in self.rows:
            self.id_to_row[row[0]] = row
        self.prices = set()
        for row in self.rows:
            self.prices.add(row[-1])
        self.rows_by_price = sorted(self.rows, key=lambda r: r[-1])
    def get_laptop_from_id(self, laptop_id):
        for row in self.rows:
            if laptop_id == row[0]:
                return row
            
        return None
    def get_laptop_from_id_fast(self, laptop_id):
        if laptop_id in self.id_to_row:
            return self.id_to_row[laptop_id]
        
        return None
    def check_promotion_dollars(self, dollars):
        for row in self.rows:
            if dollars == row[-1]:
                return True
            
        return False
    def check_promotion_dollars_fast(self, dollars):
        if dollars in self.prices:
            return True
        return False
    def find_first_laptop_more_expensive(self, target_price):
        start = 0
        end = len(self.rows_by_price) - 1
        while start < end:
            mid = (end + start) // 2
            price = self.rows_by_price[mid][-1]
            if price > target_price:
                end = mid
            elif price <= target_price:
                start = mid + 1
        price = self.rows_by_price[start][-1]
        
        if start == len(self.rows_by_price) - 1 and price < target_price:
            return -1
        
        return start
```

A nova classe, agora chamada `Climate`:

```
class Climate:
  def __init__(self, csv_filename):
    self.filename = csv_filename
    self.header = []
    self.rows = []
    
    with open(self.filename) as f:
        self.rows = list(csv.reader(f))
        self.header = self.rows[0]
        del self.rows[0]
```

Sem grandes mudanças iniciais, apenas o nome da classe e a retirada dos métodos da classe `Ìnventory`.

A instância da classe recebe a localização do arquivo csv, como podemos ver na linha abaixo:

```
class Climate:
  def __init__(self, csv_filename):
```

Guardamos este caminho num atributo, assim como declaramos os atributos `headers` e `rows`:

```
self.filename = csv_filename
self.header = []
self.rows = []
```

logo em seguida, com ajuda do `import csv`, fazemos a leitura do arquivo csv, definiremos o atributo `row` como uma lista do arquivo lido, o atributo `header` como a primeira linha do `row` e em seguida apagamos a primeira linha do `row` afim de evitar redundâncias:

```
with open(self.filename) as f:
        self.rows = list(csv.reader(f))
        self.header = self.rows[0]
        del self.rows[0]
```

## Três funções principais serão implementadas e avaliadas/comparadas o desempenho de forma similar ao que foi implementado no "Guided Project: Building Fast Queries on a CSV" (presente no curso "Algorithm Complexity"): 

### 1) Dado um id de uma mensagem no Reddit, retornar todas as informações sobre a mensagem; 

Para atender esta necessidade foi criado o método `get_comment_by_id`:

```
def get_comment_by_id(self, target_id):
    for row in self.rows:
      if row[1] == target_id:
        return row
    return None
```

Com este método, percorremos toda a classe, linha a linha, procurando no indíce `1` se o **id** é igual ao **id_alvo**

Porém a complexidade deste algoritmo é **O(N)**, onde **N** é o tamanho do arquivo. Uma das propostas do Projeto guiado é reduzir a complexidade do algoritmo.

Portanto, criamos um novo atributo chamado `id_to_row` que será um dicionário com as linhas do arquivo e a chave será o id da mensagem.

```
self.id_to_row = {}
for row in self.rows:
  self.id_to_row[row[1]] = row
```

Com isto, podemos implementar um novo método chamado `get_comment_by_id_fast` que verificará se o **id_alvo** está contido no dicionário e com isto retornar seu conteúdo:

```
def get_comment_by_id_fast(self, target_id):
    if target_id in self.id_to_row:
      return self.id_to_row[target_id]
    return None
```

No notebook é possível que tal implementação é 112 mil vezes mais rápida que a implementação anterior.

### 2) dado um limite inferior e superior da coluna "sentiment", retornar todas as mensagens com valores de sentimento entre os limites inferior e superior; 


### 3) dado um valor de parâmetro, retornar duas mensagens cuja soma do valor da coluna "score" é igual ao parâmetro. Retornar -1 se não existir. 


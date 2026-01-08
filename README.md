# Padrões Python

Nomes devem **comunicar intenção** e manter **consistência** no projeto, conforme regras adotadas.


## 1) Princípio orientador: API pública vs implementação
A PEP 8 define um princípio importante:

- Nomes que são parte **visível/publicamente suportada** de uma API devem seguir convenções que reflitam **uso**, e não detalhes internos de implementação.
- Garanta que usuários consigam distinguir claramente o que é **público** vs **interno** (ex.: `_interno`, `__all__`).


## 2) Estilos de nomes (vocabulário)

A PEP 8 lista estilos comuns de nomeação (você precisa reconhecer e aplicar o certo para cada tipo de coisa). 

| Estilo | Exemplo | Uso típico |
|---|---|---|
| `lowercase` | `json` | módulos simples, variáveis simples |
| `lower_case_with_underscores` (snake_case) | `total_items` | funções, variáveis, métodos |
| `UPPER_CASE_WITH_UNDERSCORES` | `MAX_SIZE` | constantes |
| `CapitalizedWords` (CapWords / CamelCase) | `HttpServer` | classes |
| `mixedCase` | `mixedCase` | raro; normalmente por compatibilidade/legado |

**Acrônimos em classes**: em CapWords, capitalize todas as letras do acrônimo (ex.: `HTTPServerError` melhor que `HttpServerError`). 

## 3) Underscores “especiais” (prefixos/sufixos) e o significado

A PEP 8 reconhece usos convencionais para underscores no começo/fim do nome. 

| Forma | Exemplo | Significado / uso recomendado |
|---|---|---|
| `_single_leading_underscore` | `_cache` | indicador “fraco” de **uso interno**; `from M import *` não importa nomes começando com `_` |
| `single_trailing_underscore_` | `class_` | evita conflito com **palavra reservada** |
| `__double_leading_underscore` | `__attr` | em atributos de classe: ativa **name mangling** (ex.: `__a` vira `_Classe__a`) |
| `__double_leading_and_trailing_underscore__` | `__init__` | nomes “mágicos”/especiais; **não inventar**, só usar os documentados |

## 4) Convenções prescritivas por tipo de símbolo

### 4.1. Pacotes e módulos

- **Módulos**: nomes curtos, em **minúsculas**; underscore é aceitável se melhorar legibilidade. 
- **Pacotes**: nomes curtos, em **minúsculas**; underscore é **desencorajado**. 
- Módulos de extensão C/C++ frequentemente têm **underscore inicial** quando existe um módulo Python “alto nível” por cima (ex.: `_socket`). 

### 4.2. Classes

- Classes normalmente usam **CapWords**. 
- Convenção de função (snake_case) pode aparecer como exceção quando a interface é documentada e usada primariamente como “callable”. 

### 4.3. Exceções

- Exceções são classes ⇒ **CapWords**.  
- Use sufixo **`Error`** quando de fato representar um erro.

### 4.4. Funções e variáveis

- **Funções**: minúsculas com palavras separadas por underscore quando necessário (snake_case). 
- **Variáveis**: seguem a mesma convenção das funções.  
- `mixedCase` só quando já for o estilo dominante por compatibilidade/legado.

### 4.5. Argumentos de funções e métodos

- Primeiro argumento de método de instância: **`self`**.  
- Primeiro argumento de classmethod: **`cls`**.
- Se colidir com keyword, prefira underscore no final (`class_`) em vez de abreviar ou “corromper” o nome. 

### 4.6. Métodos e atributos de instância

- Métodos: mesmas regras de função (snake_case). 
- “Não público”: **um underscore inicial** (`_interno`).  
- Para evitar colisão com subclasses: **dois underscores iniciais** (`__nome`) para acionar *name mangling* (use com parcimônia). 

### 4.7. Constantes

- Geralmente em nível de módulo: **UPPER_CASE_WITH_UNDERSCORES**.

### 4.8. Globais e `__all__`

- Para módulos pensados para `from M import *`, a PEP 8 recomenda declarar a API pública com **`__all__`** ou usar underscore para marcar globais como não-públicos. 
- Em geral, módulos devem declarar explicitamente a API pública com `__all__`; `__all__ = []` indica “sem API pública”.  
- Mesmo com `__all__`, nomes internos devem continuar com underscore inicial. 

### 4.9. Herança e “público vs não público”

- Se estiver em dúvida, prefira **não público** (mais fácil tornar público depois do que “quebrar” uma API pública).   
- A PEP 8 evita o termo “privado” (não há privacidade real no Python sem esforço extra).

## 5) Nomes a evitar + compatibilidade ASCII/Unicode

### 5.1. Nomes a evitar

- Não use `l`, `O` ou `I` como nome de variável de **um caractere** (confunde com `1` e `0`).

### 5.2. ASCII Compatibility (stdlib) e Unicode em identificadores

- A PEP 8 afirma que identificadores usados na **stdlib** precisam ser ASCII-compatíveis conforme a política da PEP 3131.   
- A PEP 3131 define a política: na stdlib, identificadores devem ser **ASCII-only** e “idealmente” usar palavras em inglês quando possível (com exceções específicas).
## 6) Type hints: nomes recomendados para TypeVars (PEP 8 + PEP 484)

A PEP 8 recomenda:

- Type variables (PEP 484) normalmente usam **CapWords**, preferindo nomes curtos como `T`, `AnyStr`, `Num`. 
- Para variância, use sufixos:
  - `_co` para `covariant=True`
  - `_contra` para `contravariant=True` 

A PEP 484 também descreve o uso de `TypeVar` com `covariant=True` / `contravariant=True` e exemplifica esse padrão. 

## 7) Docstrings (PEP 257) e o impacto prático em “nomeação”

Docstrings não são “nomeclatura”, mas são parte da **interface pública**: ajudam a documentar o que seu nome representa.

- Docstring é uma string literal como **primeiro statement** de módulo/função/classe/método e vira `__doc__`.   
- Em geral: módulos devem ter docstrings; funções e classes exportadas também; métodos públicos (incluindo `__init__`) idem.   
- A PEP 8 recomenda docstrings para tudo que é público e referencia a PEP 257. 
# Tabelas-resumo rápidas

## A) “O que usar onde”

| Categoria | Padrão | Exemplo |
|---|---|---|
| Pacote | `lowercase` curto (underscore desencorajado) | `meupacote` |
| Módulo | `lowercase` curto; underscore ok | `data_loader.py` |
| Classe | `CapWords` | `UserService` |
| Exceção | `CapWords` + `Error` | `ValidationError` |
| Função | `snake_case` | `load_data()` |
| Variável | `snake_case` | `total_count` |
| Constante | `UPPER_CASE_WITH_UNDERSCORES` | `MAX_SIZE` |
| Método | `snake_case` | `save_changes()` |

## B) “Underscores especiais”

| Convenção | Quando usar |
|---|---|
| `_nome` | interno / não público |
| `nome_` | evitar conflito com keyword |
| `__nome` | evitar colisão em herança (name mangling) |
| `__nome__` | só nomes especiais documentados |

# Tabela final: resumo completo 

| O que você está nomeando | Convenção | Exemplos | Observações |
|---|---|---|---|
| **Pacote** | `lowercase` curto (underscore desencorajado) | `mypkg`, `core` | pacotes em minúsculas  |
| **Módulo** | `lowercase` curto; underscore ok | `utils.py`, `data_loader.py` | wrapper C pode ser `_socket` |
| **Classe** | `CapWords` | `UserService`, `HTTPServerError` | acrônimos em maiúsculas  |
| **Exceção** | `CapWords` + `Error` | `ValidationError` |  |
| **Função** | `snake_case` | `load_data()` | `mixedCase` só por legado  |
| **Variável** | `snake_case` | `total_count` | mesmas regras de função |
| **Constante** | `UPPER_CASE_WITH_UNDERSCORES` | `MAX_SIZE` | normalmente em nível de módulo |
| **Método / atributo** | `snake_case` | `save_changes`, `user_id` | regras de função |
| **Interno / não público** | `_snake_case` | `_cache` | indica “interno”  |
| **Evitar keyword** | `name_` | `class_`, `from_` | melhor que abreviar  |
| **Name mangling** | `__name` | `__state` | útil em herança; não é “privado real”  |
| **Dunder** | `__name__` | `__init__`, `__len__` | não inventar  |
| **API pública do módulo** | `__all__` | `__all__ = ["Foo"]` | documentado = público; `_` continua interno  |
| **Argumentos padrão** | `self` / `cls` | `def m(self)` / `def m(cls)` | convenção explícita  |
| **TypeVar** | `CapWords` curto; `_co/_contra` | `T`, `AnyStr`, `VT_co` | variância via sufixos  |
| **Docstrings** | PEP 257 | `"""Resumo..."""` | docstring = 1º statement e vira `__doc__`  |
| **Evitar nomes** | não usar `l`, `O`, `I` sozinho | `l`/`O`/`I` | confusão visual  |
| **ASCII/Unicode (stdlib)** | stdlib: ASCII-only | — | política descrita na PEP 3131  |

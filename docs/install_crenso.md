[⬅ Voltar](../README.md)

# Guia de instalação

Para usar o CRENSO, você precisar ter o CREST e o CENSO instalados. Mas ambos fazem parte do pacote xTB, por isso você primeiro precisa instalar o xTB.

### xTB

O [xTB](https://xtb-docs.readthedocs.io/en/latest/) pode ser instalado de três formas:

- Com o conda.
- Baixando um binário pré compilado.
- Compilando o xTB a partir do código fonte.

A forma mais simples, é instalar pelo Conda, mas ao fazer isso, você sempre vai precisar ativar o ambiente do Conda para realizar os cálculos (o que não é um problema, com literalmente um comando você consegue ativar o ambiente).

Ao baixar um binário pré compilado, você poderá usar o xTB sem precisar ativar um ambiente do Conda, mas você pode ter que realizar algumas configurações mais complexas, o que pode ser um pouco mais desafiador para quem não tem conhecimento de linux.

Compilar o xTB a partir do código fonte é a melhor opção, mas é a forma de instalar mais complicada de todas, instalar o xTB pelo conda pode ser a melhor opção caso você não tenha conhecimento de linux e compiladores.

Após instalar, é bom conferir se o xTB foi corretamente instalado, com o comando.

```bash
xtb --version

-----------------------------------------------------------      
|                   =====================                   |     
|                           x T B                           |     
|                   =====================                   |     
|                         S. Grimme                         |     
|          Mulliken Center for Theoretical Chemistry        |     
|                    University of Bonn                     |     
-----------------------------------------------------------      

* xtb version 6.6.1 (8d0f1dd) compiled by 'conda@1efc2f54142f' on 2023-08-01

normal termination of xtb
```

### CREST

O [CREST](https://crest-lab.github.io/crest-docs/page/installation) pode ser instalado de duas formas:

- A partir de um binário pré compilado.
- Compilando a partir do código fonte.

A forma mais simples é baixar o arquivo pré compilado, o guia de instalação do CREST explica passo a passo como instalar.

Após instalar, é bom verificar se o CREST foi corretamente instalado, com o comando.

```bash
crest --version
```

### CENSO

Assim como o CREST, o [CENSO](https://xtb-docs.readthedocs.io/en/latest/CENSO_docs/censo_setup.html) pode ser instalado de duas formas:

- A partir de um binário pré compilado.
- Compilando a partir do código fonte.

A forma mais simples é baixar o arquivo pré compilado, o método de instalação é basicamente o mesmo do CREST.

Após instalar, é bom verificar se o CREST foi corretamente instalado, com o comando.

```bash
censo --version
```

### ORCA

O CENSO trabalha com conjuntos com outros programas, como o ORCA, TURBOMOLE, COSMOtherm.

No fluxo de trabalho original, do artigo do grupo do professor Stefan Grimme, o CENSO é usado em conjunto com o COSMOtherm, usando o modelo de solvatação COSMO-RS que está disponível nele, a questão é que é um programa pago, com licença anual.

Dessa forma, optamos por usar o ORCA, com o modelo de solvatação SMD, que você pode usar gratuitamente, e que consegue nos dar bons resultados.

O guia de instalação do ORCA pode ser encontrado [aqui](https://www.orcasoftware.de/tutorials_orca/first_steps/install.html).

Após instalar, é bom verificar se o ORCA foi corretamente instalado, com o comando.

```bash
orca --version
```


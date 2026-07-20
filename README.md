# CS 1.6 competitivo com bots YaPB

Configurações para jogar **Counter-Strike 1.6 em partidas 5 contra 5**, com parâmetros inspirados no competitivo clássico do CS:GO e bots controlados pelo **YaPB**.

O projeto contém duas configurações:

| Arquivo | Equipes criadas | Equipe do jogador |
|---|---|---|
| `ct_competitive.cfg` | 4 bots CT contra 5 bots TR | CT |
| `tr_competitive.cfg` | 5 bots CT contra 4 bots TR | TR |

Ao entrar na equipe indicada, a partida fica com **5 CT contra 5 TR**.

## Regras configuradas

As duas CFGs aplicam:

- 30 rounds no tempo regulamentar;
- vitória ao atingir 16 rounds;
- dinheiro inicial de `$800`;
- round de aproximadamente 1 minuto e 55 segundos;
- 15 segundos de congelamento no início do round;
- 20 segundos de tempo de compra;
- C4 configurada para 40 segundos;
- fogo amigo ativado;
- comunicação entre equipes desativada;
- balanceamento automático desativado;
- câmera restrita após a morte;
- reinício de 3 segundos antes do pistol round;
- bots na dificuldade profissional.

> [!IMPORTANT]
> As CFGs reproduzem somente as regras que podem ser aplicadas diretamente pelos comandos do CS 1.6 e do YaPB. Elas **não fazem troca automática de lados**, **não executam prorrogação**, **não criam pausas técnicas** e **não reproduzem integralmente a economia do CS:GO**.

## Dependências

### Obrigatórias

1. **Counter-Strike 1.6**
   - Instalação do jogo pela Steam ou um servidor HLDS compatível.
   - Pode ser usado em servidor local, servidor de escuta (*listen server*) ou servidor dedicado.

2. **YaPB**
   - Necessário porque as CFGs utilizam comandos iniciados por `yb`, como:
     - `yb kickall`;
     - `yb add`;
     - `yb_difficulty`;
     - `yb_autovacate`;
     - `yb_join_after_player`.

3. **Waypoints do YaPB para o mapa**
   - O mapa escolhido precisa possuir waypoints compatíveis.
   - Mapas oficiais normalmente já são atendidos pelo pacote do YaPB.
   - Sem waypoints, os bots podem não entrar corretamente ou ficar sem navegação.

4. **Servidor com pelo menos 10 vagas**
   - São necessários nove bots e um jogador humano.
   - Em servidor dedicado, configure o número de jogadores na inicialização, por exemplo:

```bash
-maxplayers 10
```

### Opcionais

#### Metamod-P

O YaPB pode funcionar como biblioteca independente ou como plugin do Metamod. O Metamod não é obrigatório para estas CFGs, mas é recomendado quando o servidor também utiliza outros plugins.

Exemplos de entrada no arquivo `cstrike/addons/metamod/plugins.ini`:

**Windows:**

```ini
win32 addons/yapb/bin/yapb.dll
```

**Linux:**

```ini
linux addons/yapb/bin/yapb.so
```

#### AMX Mod X

O AMX Mod X **não é necessário** para executar os arquivos deste projeto.

Ele passa a ser útil caso você queira implementar recursos adicionais, como:

- troca automática de lados após 15 rounds;
- prorrogação;
- placar de primeiro e segundo tempo;
- pausas técnicas;
- comandos de pronto;
- controle de capitães e equipes;
- restauração de rounds;
- sistema competitivo mais completo.

## Instalação

### 1. Localize a pasta do jogo

Na instalação padrão da Steam no Windows, a pasta normalmente é:

```text
C:\Program Files (x86)\Steam\steamapps\common\Half-Life\cstrike
```

Em algumas instalações, ela pode estar em outra biblioteca da Steam:

```text
<PASTA_DA_BIBLIOTECA>\steamapps\common\Half-Life\cstrike
```

No Linux, um caminho comum é:

```text
~/.local/share/Steam/steamapps/common/Half-Life/cstrike
```

### 2. Instale o YaPB

Extraia o pacote do YaPB dentro da pasta `cstrike`.

Após a instalação, a estrutura deve conter algo semelhante a:

```text
cstrike/
└── addons/
    └── yapb/
        ├── bin/
        ├── conf/
        └── ...
```

O YaPB pode ser carregado diretamente pelo jogo ou pelo Metamod. Consulte a documentação oficial indicada no final deste README para o método adequado ao seu sistema.

### 3. Copie as configurações

Coloque os dois arquivos diretamente na pasta `cstrike`:

```text
cstrike/
├── ct_competitive.cfg
├── tr_competitive.cfg
└── ...
```

## Como usar

### Servidor local

1. Abra o Counter-Strike 1.6.
2. Crie uma nova partida.
3. Escolha preferencialmente um mapa de bomba, como `de_dust2`, `de_inferno` ou `de_nuke`.
4. Abra o console.
5. Execute uma das configurações.

Para jogar de CT:

```cfg
exec ct_competitive.cfg
```

Depois, entre na equipe **Contra-Terrorista**. A CFG cria quatro bots CT e cinco bots TR.

Para jogar de TR:

```cfg
exec tr_competitive.cfg
```

Depois, entre na equipe **Terrorista**. A CFG cria cinco bots CT e quatro bots TR.

### Servidor dedicado

No console do servidor:

```cfg
exec ct_competitive.cfg
```

ou:

```cfg
exec tr_competitive.cfg
```

Usando RCON a partir do cliente:

```cfg
rcon exec ct_competitive.cfg
```

ou:

```cfg
rcon exec tr_competitive.cfg
```

O jogador deve entrar na equipe correspondente ao arquivo executado.

## Iniciar outra partida

Para remover todos os bots atuais:

```cfg
yb kickall instant
```

Depois, carregue novamente a configuração desejada:

```cfg
exec ct_competitive.cfg
```

ou:

```cfg
exec tr_competitive.cfg
```

Para trocar o mapa antes de recarregar a CFG:

```cfg
changelevel de_inferno
```

Depois que o mapa carregar, execute novamente o arquivo desejado.

## Alterar a dificuldade dos bots

O valor atual é `3`, correspondente à dificuldade profissional.

Valores usados pelas CFGs:

| Valor | Dificuldade |
|---:|---|
| `2` | Normal |
| `3` | Profissional |
| `4` | Extremamente difícil |

Para alterar a dificuldade, edite:

```cfg
yb_difficulty 3
```

Também altere o **primeiro número** de cada comando `yb add`.

Exemplo na dificuldade extremamente difícil:

```cfg
yb_difficulty 4
yb add 4 0 2 0
```

Os parâmetros utilizados em `yb add` seguem esta estrutura:

```text
yb add <dificuldade> <personalidade> <equipe> <classe>
```

Nas CFGs:

- equipe `1` representa Terroristas;
- equipe `2` representa Contra-Terroristas;
- personalidade `0` usa a seleção padrão;
- classe `0` usa uma classe aleatória ou padrão.

## Personalização das regras

Os principais valores podem ser alterados nos dois arquivos:

```cfg
mp_freezetime 15
mp_roundtime 1.92
mp_buytime 0.3333
mp_c4timer 40

mp_startmoney 800
mp_maxrounds 30
mp_winlimit 16

mp_friendlyfire 1
```

### Conversão de tempo

No CS 1.6, alguns tempos são informados em minutos decimais:

- `mp_roundtime 1.92` equivale a aproximadamente 1 minuto e 55 segundos;
- `mp_buytime 0.3333` equivale a aproximadamente 20 segundos.

## Troca de lados

Os arquivos não realizam troca automática no intervalo.

Para fazer a troca manual após 15 rounds:

1. Anote o placar.
2. Remova os bots:

```cfg
yb kickall instant
```

3. Troque para o lado oposto.
4. Execute a CFG correspondente ao novo lado:

```cfg
exec tr_competitive.cfg
```

ou:

```cfg
exec ct_competitive.cfg
```

5. Continue a partida controlando manualmente o placar total.

Para uma troca de lados e prorrogação totalmente automáticas, será necessário utilizar um plugin específico para AMX Mod X ou outra solução de gerenciamento competitivo.

## Solução de problemas

### `Unknown command: yb`

O YaPB não foi carregado.

Verifique:

- se a pasta `cstrike/addons/yapb` existe;
- se a instalação independente foi configurada corretamente; ou
- se o YaPB está registrado no `plugins.ini` do Metamod.

Caso esteja usando Metamod, execute no console do servidor:

```cfg
meta list
```

O YaPB deve aparecer na lista de plugins carregados.

### A CFG não foi encontrada

Confirme se os arquivos estão diretamente em:

```text
Half-Life/cstrike/
```

Depois, execute:

```cfg
exec ct_competitive.cfg
```

Não coloque os arquivos dentro de `addons/yapb/conf`, pois eles são configurações do servidor, não configurações internas do bot.

### Os bots não se movimentam corretamente

O mapa provavelmente não possui waypoints compatíveis com o YaPB.

Teste primeiro em um mapa oficial, como:

```text
de_dust2
de_inferno
de_nuke
de_train
```

### A partida não fica 5 contra 5

Verifique:

- se o servidor possui pelo menos 10 vagas;
- se você entrou na equipe correta;
- se já existiam jogadores ou bots antes da execução;
- se a CFG terminou de adicionar todos os bots.

Recarregue a configuração:

```cfg
yb kickall instant
exec ct_competitive.cfg
```

ou:

```cfg
yb kickall instant
exec tr_competitive.cfg
```

### Um bot foi removido quando um jogador entrou

As CFGs definem:

```cfg
yb_autovacate 0
```

Isso impede que o YaPB remova automaticamente um bot para abrir espaço. Garanta que o servidor já tenha vagas suficientes antes de conectar novos jogadores.

### A troca de lados não aconteceu

Esse comportamento é esperado. O CS 1.6 padrão não executa a estrutura completa de primeiro tempo, intervalo, segundo tempo e prorrogação apenas com estas CFGs.

## Observações

- Recomenda-se usar mapas do tipo `de_`, pois as regras incluem tempo de C4.
- Execute a CFG novamente sempre que trocar de mapa.
- Caso vários jogadores humanos entrem, ajuste manualmente a quantidade de bots.
- Os comandos foram preparados para uma partida com apenas um jogador humano.
- Faça backup de qualquer arquivo antes de personalizar as configurações.

## Documentação de referência

- [Documentação oficial do YaPB](https://yapb.readthedocs.io/en/latest/)
- [Instalação oficial do YaPB](https://yapb.readthedocs.io/en/latest/installation.html)
- [Uso e comandos do YaPB](https://yapb.readthedocs.io/en/latest/botusage.html)
- [Documentação do Metamod-P](https://metamod-p.sourceforge.net/doc/html/metamod.html)
- [Documentação do AMX Mod X](https://www.amxmodx.org/doc/)

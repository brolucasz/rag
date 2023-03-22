# Como alterar o nível máximo de 99 para 255 no rAthena

## Para adicionar tabelas de EXP para o nível base 255 a partir do nível 99, você precisa apenas do Notepad++ para editar o arquivo job_exp.txt. Siga estas instruções rápidas:

  1. Abra o job_exp.txt ([pre](https://github.com/rathena/rathena/blob/master/db/pre-re/job_exp.txt) | [re](https://github.com/rathena/rathena/blob/master/db/re/job_exp.txt)) usando o Notepad++.

  2. Crie um novo arquivo em branco.
  3. Adicione 156 linhas em branco, *porque para adicionar o nível máximo de 99 para 255 são necessárias 156 entradas adicionais de EXP ** ![156 empty lines](http://pservero.com/wp-content/uploads/2016/12/Screenshot_221.png)
  4. No arquivo em branco e na primeira linha, clique em **Editar** menu > escolha **Editor de Coluna...**, ou pressione **ALT+C** ![Open Column Editor](http://pservero.com/wp-content/uploads/2016/12/Screenshot_210.png)
  5. Agora, selecione a opção de marca de seleção para **Número a inserir.**
  6.Como exemplo, a última EXP para o nível 99 é 4.000.000, e você deseja adicionar um intervalo de EXP de 70.000 do nível 100 ao 255. Então escreva 4.070.000 em **Número Inicial.**
  7. Em seguida, escreva **70.000** em **Incrementar por**, pressione OK. ![Column Editor](http://pservero.com/wp-content/uploads/2016/12/Screenshot_217.png)
  8. Novos valores de EXP do nível 100 ao 255 são gerados em 156 linhas! [EXP result](http://pservero.com/wp-content/uploads/2016/12/Screenshot_214.png)
  9.Para garantir que não haja espaço extra, clique em **Editar** menu > **Operações em Branco** > ** Aparar Espaço Final ** ![Remove trailing spaces](http://pservero.com/wp-content/uploads/2016/12/Screenshot_215.png)
  10. Pressione CTRL+H para substituir as 156 linhas de EXP do nível 100 ao 255 para o formato CSV.
  11. Em **Localizar**, escreva `\r\n`para EOL DOS/Windows ou `\n` para UNIX EOL.
  12. Em **Substituir por**, escreva  `,` (vírgula) ![Substitua EOFs por vírgula](http://pservero.com/wp-content/uploads/2016/12/Screenshot_219.png)
  13. Pressione **Substituir tudo.**
  14. Selecione todas as entradas, copie-as! [Extra EXP table for level 255](http://pservero.com/wp-content/uploads/2016/12/Screenshot_220.png)
  15. Cole no arquivo job_exp.txt antes do 99999999 e adicione uma vírgula extra. O 99.999.999 é a EXP máxima para o personagem no nível máximo.
  16.	Agora, altere o 99 para 255 para o nível máximo da base! ![Novo job_exp.txt até o nível 255](http://pservero.com/wp-content/uploads/2016/12/Screenshot_218.png)


## Edite o arquivo src
  1. Abra [src/map/map.h](https://github.com/rathena/rathena/blob/master/src/map/map.h)
  2. Encontre `#define MAX_LEVEL` ![Find MAX_LEVEL in src/map/map.h](http://pservero.com/wp-content/uploads/2016/12/Screenshot_222.png)
  3. Altere para `#define MAX_LEVEL 255` para que o arquivo job_exp.txt esteja pronto para o Nível Máximo 255! ![Change Max Level to 255](http://pservero.com/wp-content/uploads/2016/12/Screenshot_223.png)
  4. Recompile o servidor de mapas.


## Recompile o servidor de mapas. [conf/battle/client.conf](https://github.com/rathena/rathena/blob/master/conf/battle/client.conf)

## O cliente não exibe aura com base no Nível Máximo
Você deve saber que o cliente possui uma verificação 'codificada' para marcar se o jogador tem aura ou não. Por padrão, o cliente define a aura para o nível 99 ou normal e 2ª-trans classes, 150 para 3ª classes em clientes mais antigos, 160 (Kagerou/Oboro, Rebelião, Superaprendiz Expandido) e 175 para clientes mais recentes.
Você pode enganar o cliente usando 'ei, o nível máximo do meu servidor é 255, então não defina a aura para personagens abaixo do nível 255' editando estas 2 configurações em  [conf/battle/client.conf](https://github.com/rathena/rathena/blob/master/conf/battle/client.conf)

```
// Valor máximo permitido para o 'nível' que pode ser enviado em pacotes de unidade.
// Use em conjunto com a configuração aura_lv para dizer exatamente quando mostrar a aura.
// NOTA: Você também precisa ajustar o cliente se quiser que isso funcione.
// NOTA: O padrão é 99. Valores acima de 127 provavelmente se comportarão incorretamente.
// NOTA: Se você não sabe o que isso faz, não mude isso!!!
max_lv: 99

// Nível necessário para exibir uma aura.
// NOTA: Isso pressupõe que enviar max_lv para o cliente exibirá a aura.
// NOTA: aura_lv não deve ser menor que max_lv.
// Exemplo: Se max_lv for 99 e aura_lv for 150, personagens com nível 99~149
//          serão enviados como sendo todos de nível 98 e somente personagens com nível
//          150 ou mais serão relatados como tendo nível 99 e mostrarão uma aura.
aura_lv: 99

```
Lembre-se de que não há truque perfeito.

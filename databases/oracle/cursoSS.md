
# Anotações 'Curso Oracle Completo' do instrutor: Sandro Servino

## Instalação

- RAC significa "Real Application Clusters" e é uma tecnologia da Oracle que permite criar um ambiente de cluster de banco de dados altamente disponível e escalável. Em um ambiente RAC, múltiplas instâncias do banco de dados Oracle são executadas em diferentes servidores (nós) que estão interconectados e compartilham o mesmo armazenamento de dados. 

- Desktop class

    A classe Desktop é a configuração mínima do seu banco de dados Oracle. 

- Server Class

    A classe Server, como o nome sugere, é uma instalação mais avançada do Oracle DB.

- Usuario de servico que rodará o Oracle no windows:

  
  - User Virtual Account: Essa opção refere-se a uma conta de usuário virtual gerenciada pelo Windows. As contas de usuário virtual são contas de serviço especiais que o Windows cria automaticamente para executar serviços específicos. Se você selecionar esta opção, o Oracle utilizará uma conta de usuário virtual para executar o serviço do banco de dados. Essa conta é geralmente criada com permissões limitadas, específicas para o serviço, e é gerenciada pelo sistema operacional.

  - Use Existing Windows User: Essa opção permite que você especifique 
uma conta de usuário já existente no Windows para executar o 
serviço do Oracle. Se você escolher esta opção, precisará fornecer 
os detalhes (nome de usuário e senha) da conta existente 
para o Oracle usar como a conta de serviço.

  - Create New Windows User: Essa opção permitirá que você crie uma nova 
conta de usuário no Windows durante o processo de 
instalação do Oracle. O Oracle usará essa nova conta como a conta 
de serviço para executar o banco de dados.

  - Use Windows Built-in Account: Essa opção permite que você use uma 
conta de usuário interna do Windows para executar o 
serviço do Oracle. Geralmente, essas contas internas são contas de 
serviço especiais do Windows, como "LocalSystem" ou 
"Network Service". Essas contas têm permissões específicas 
no sistema e são gerenciadas pelo próprio Windows.

- Informações sobre o banco de dados que está sendo criado:

  - Global Database Name:

    O Global Database Name é o nome completo e exclusivo do banco de dados 
Oracle em todo o sistema de rede. Ele é usado para 
identificar unicamente o banco de dados Oracle em um ambiente de rede 
e é composto por duas partes: o nome do banco de dados 
e o domínio. Por exemplo, no Global Database Name "orcl.example.com", "ORCL"
 é o nome do banco de dados, e "example.com" é o domínio.
O Global Database Name é importante para garantir a resolução correta 
de nomes de banco de dados em uma rede onde podem 
existir vários bancos de dados Oracle. 
Ele é usado para localizar e conectar-se ao banco de dados 
em diferentes computadores e servidores na rede.

  - Oracle System Identifier (SID):
    O Oracle System Identifier (SID) é um identificador único e exclusivo 
atribuído a uma instância do banco de dados Oracle. 
Uma instância é a combinação de memória e processos que representam 
o funcionamento do banco de dados em um servidor específico. 
O SID é usado pelo sistema operacional para identificar cada 
instância do banco de dados em um servidor.

    Banco de dados para o Oracle é um conjunto de arquivos fisicos 
que ficam em um maquina fisica ou virtual. Pode exisir em um mesmo servidor 
1 BANCO DE DADOS
OU VARIOS BANCOS DE DADOS, O QUE CHAMAMOS DE PDB (Pluggable Database Name). 
Cada PDB pode ter varios users/schemas e cada um destes users/schemas 
é o que conhecemos
como databases em outros SGBD como sql server, mysql e postgresql.

  - Pluggable Database Name:
O Pluggable Database Name é relevante apenas em ambientes multitenant 
(a partir do Oracle Database 12c), onde um único banco de dados "contêiner" 
(CDB - Container Database) pode conter vários bancos de dados 
"plugáveis" (PDBs - Pluggable Databases). 
Nesse modelo, o CDB funciona como um contêiner para PDBs, permitindo 
que vários bancos de dados sejam consolidados em uma única instância 
do Oracle.
Cada PDB possui um nome exclusivo, e esse nome é chamado de 
Pluggable Database Name. No exemplo mencionado,
 o Pluggable Database Name é "orclpdb".

    Por padrão, é definido como “ORCLPDB” se você quiser, pode alterá-lo. 
Esse banco de dados conectável servirá como modelo para todos o
s PDBs que você criará no futuro.

Arquitetura Oracle: D:\Backups\videoscursoudemyBK\ORACLE\images\ARQUITETURA
CDB E PDBS        : D:\Backups\videoscursoudemyBK\ORACLE\images\CDBXPDB


- Usuarios do banco de dados

  - SYS: O usuário SYS é o superusuário do banco de dados, com privilégios 
administrativos completos. A senha para este usuário deve ser forte, 
com uma combinação de letras maiúsculas, letras minúsculas, 
números e caracteres especiais. Evite usar senhas óbvias ou fáceis 
de adivinhar.

  - SYSTEM: O usuário SYSTEM também é um usuário administrativo com 
privilégios elevados, embora não possua todos os privilégios do 
usuário SYS. A senha para o usuário SYSTEM também deve ser forte 
e diferente da senha do usuário SYS.

  - PDBADMIN: O usuário PDBADMIN é utilizado para administração de um 
PDB (Pluggable Database) específico, no caso do PDB que sera instalado 
por padrao.
O Oracle 21c permite criar várias Pluggable Databases dentro de 
um único container 
de banco de dados (CDB). A senha do usuário PDBADMIN também deve ser 
forte e diferente das senhas dos usuários SYS e SYSTEM.

- Verificar se o listener

  - Windows service = OracleOraDB21Home1TNSListener
  - Dos = lsnrctl status

- Forçar start do Listener

  lsnrctl start

- Se erro, verificar o arquivo listener.ora em: 
(C:\app\administrator\product\21\dbhome_1\network\admin\sample\listener.ora) 

  Exemplo:

  SID_LIST_LISTENER =
    (SID_LIST =
      (SID_DESC =
        (SID_NAME = CLRExtProc)
        (ORACLE_HOME = C:\app\administrator\product\21\dbhome_1)
        (PROGRAM = extproc)
        (ENVS = "EXTPROC_DLLS=ONLY:C:\app\administrator\product\21\dbhome_1\bin\oraclr.dll")
      )
    )

  LISTENER =
    (DESCRIPTION_LIST =
      (DESCRIPTION =
        (ADDRESS = (PROTOCOL = TCP)(HOST = WIN-K1S9FADUUH0)(PORT = 1521))
        (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
      )
    )


- Teste de conexão com o client do Oracle. 

  - sqlplus / as sysdba
  - sqlplus sys as sysdba

  - Executar: 
    
    select instance_name, status from v$instance;

- DESLIGANDO O SERVIDOR ORACLE: SHUTDOWN

  O comando "shutdown;" no Oracle é usado para encerrar o banco de dados 
Oracle de forma controlada. 


  shutdown <<mode>>;

  - Modos:
  
    - normal: Em um encerramento normal, o Oracle Database espera que todos os usuários 
atualmente conectados se desconectem e não permite novas conexões antes de encerrar. 
Este é o modo padrão.
    
    - immediate: Em um encerramento imediato, o Oracle Database encerra e faz rollback 
em transações ativas e não comitadas, desconecta clientes e desliga.
    
    - abort: Em um encerramento abortado, o Oracle Database encerra transações ativas 
e desconecta usuários; ele não faz rollback nas transações. O banco de dados executa rollbacks na próxima vez que for iniciado. 
Use este modo apenas em emergências porque ja tive problema com banco
de dados depois quando dei startup.

  - Quando você executa esse comando, o seguinte acontece:

    - "Database closed": Isso significa que todas as conexões de usuários 
ao banco de dados são encerradas. 
O banco de dados não aceitará mais novas conexões a partir deste momento.

    - "Database dismounted": O banco de dados é desmontado. Isso significa que os 
arquivos de dados e de controle do banco de 
dados não estão mais acessíveis para a instancia Oracle. 
Os arquivos do banco de dados não estão mais em uso pelo Oracle.

    - "ORACLE instance shut down": A instância do Oracle 
(os processos que estao na memoria em execução que controla o banco de dados ) 
sao encerrados. Isso significa que o banco de dados não está mais 
em execução no servidor.

- Iniciar o ORACLE

  No sqlplus, execute: 
  
  startup;

    ORACLE instance started.
    Database mounted.
    Database opened.


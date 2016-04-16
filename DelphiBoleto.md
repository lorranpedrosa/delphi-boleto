# DELPHI-BOLETO #

Pacote de componentes para cobrança bancária em Delphi (geração de boletos bancários completos incluindo código de barras, geração de arquivo remessa e processamento de arquivo retorno)

AUTOR ORIGINAL DO PACOTE DE COMPONENTES DELPHI-BOLETO
Autor : Genilton Barbosa
E-mail: genilton@hotmail.com
Site  : www.gbimoveis.com/genilton


1. DESCRIÇÃO

O Delphi-Boleto é composto por 2 componentes:

1.1 TGBTITULO:
a - Imprime boleto bancário (incluindo código de barras) para diversos bancos (usa o QuickReport);
b - Gera imagem completa do boleto (TImage);
c - Envia imagem do boleto por e-mail (usa o componente TNMSMTP);
d - Gera imagem da Ficha de Compensação - TImage (aquela parte que tem o código de barras). Nesse caso, você pode criar um relatório incluindo os dados da cobrança e incluir a Ficha de Compensação no rodapé do relatório. Algo parecido com o que é feito com as contas de telefone, por exemplo.

1.2 TGBCOBRANCA:
a - Gera arquivo remessa para o banco (Suponha um componente gbCobranca1 do tipo TgbCobranca. Você lê o seu banco de dados e atribui o valor de cada cobrança a um componente TgbTitulo - Ex.: gbTitulo1. Depois, usa gbCobranca1.Titulos.Add(gbTitulo1) para inserir o título na coleção. Para terminar, você usa gbCobranca1.GerarRemessa);
b - Lê arquivo retorno - recebido do banco (gbCobranca1.LerRetorno). Depois, para cada titulo da coleção TgbCobranca, você procura o NossoNumero no banco de dados (gbCobranca1.Titulos[i](i.md).NossoNumero) e atribui aos campos do banco de dados os valores informados pelo banco: descontos, acréscimos, juros, data do recebimento, etc... (gbCobranca1.Titulos[i](i.md).ValorDescontos, gbCobranca1.Titulos[i](i.md).ValorAcrescimos ....)

###  ###

2. LICENÇA:

Este componente foi desenvolvido por GENILTON BARBOSA (www.gbimoveis.com/genilton) e é aperfeiçoado com apoio dos membros do grupo DELPHI-BOLETO. O grupo DELPHI-BOLETO foi criado e é coordenado por Genilton Barbosa.
O componente é open source e você poderá alterá-lo e distribuí-lo livremente desde que inclua na distribuição o texto integral desta licença, sem alterações.
Para contribuir com a comunidade Open Source e com o grupo Delphi-Boleto em particular, peço que todas as contribuições (sugestões, alterações no código, etc...) sejam enviadas para o grupo Delphi-Boleto.
Inscreva-se gratuitamente no grupo Delphi-Boleto e você terá acesso a todas as versões atualizadas dos componentes, além de poder contribuir para o aperfeiçoamento dos componentes e participar da nossa lista de discussão sobre eles. Para se cadastrar no grupo, visite o site www.gbimoveis.com/genilton.

###  ###

3. INSTALAÇÃO

2.1. Descompacte o arquivo delphi\_boleto\_2\_1.zip;
2.2. Usando o Delphi, abra o arquivo gbCobX.dpk, onde X é a versão do Delphi que você usa (4, 5, 6 ou 7);
2.3. Clique na opção Compilar;
2.4. Clique na opção Instalar;
2.5. Adicione $DELPHI\Source\ToolsAPI no caminho de pesquisa do Delphi (menu Tools / Environment Options / Library / Search Path);
2.6. Antes de usar o programa de demonstração que está no subdiretório DEMO, você deverá criar o alias GBFINANCAS e apontá-lo para o subdiretório DEMO\DADOS.

Será criada uma paleta chamada GBSOFT na barra de componentes do Delphi, com 2 componentes: TgbTitulo e TgbCobranca.

OBS.: O componente TgbCobranca (gera remessa e lê retorno de cobrança) não está disponível para Delphi 4.

###  ###

4. COMPONENTES

4.1 TGBTITULO

Representa um título e todas as rotinas associadas}

4.1.1 Propriedades

TipoOcorrencia : TTipoOcorrencia; {Indica o que deverá ser feito com o título: registrar, cancelar, baixa, etc...)
MotivoRejeicaoComando : string; {Caso uma ocorrência não seja aceita pelo bancos, aqui mostra qual foi o motivo)
LocalPagamento : string; {Local onde o título deverá ser pago. Ex.: 'Pagável em qualquer banco até o vencimento'}
Cedente : TgbCedente; {Aquele que emitiu o título}
Sacado  : TgbPessoa; {Cliente que deverá pagar a conta}
SeuNumero, {Número que identifica o título na empresa}
NossoNumero, {Número que identifica o título no banco}
DigitoNossoNumero,
Carteira: string; {Carteira do título, conforme informado pelo banco. Ex.: 175, SR}
AceiteDocumento : TAceiteDocumento;
DataProcessamento : {Data em que o título foi impresso}
DataDocumento, {Data do documento que originou a cobrança (nota fiscal, etc...}
DataVencimento, {Data do vencimento do título}
DataRecebimento, {Data em que o título foi pago pelo sacado}
DataCredito : TDateTime; {Data em que o banco liberará o dinheiro para o cedente}
DataAbatimento : TDateTime; {Data máxima para conceder abatimentos ao sacado}
DataDesconto : TDateTime; {Data máxima para conceder desconto ao sacado}
DataMoraJuros : TDateTime; {Data a partir da qual serão cobrados juros}
DataProtesto : TDateTime; {Data em que o título deverá ser protestado}
DataBaixa : TDateTime; {Data em que o título deverá ser baixado (desativado))
ValorDocumento, {Valor do título}
ValorDespesaCobranca : Currency; {Valor cobrado pelo banco para fazer a cobrança do título}
Instrucoes : TStringList; {Informações que serão incluídas na área de instruções do boleto}
EmissaoBoleto : TEmissaoBoleto; {Indica quem deverá imprimir / enviar o título: cliente ou banco}
ImagemBoleto : TImage; {Imagem do boleto como seria impresso}
ImagemFichaCompensacao : TImage; {Imagem da Ficha de Compensação (parte do boleto que contém o código de barras). Pode ser usada para geração de boletos personalizados. Inclua os dados desejados no relatório e coloque a imagem no rodapé do relatório}

{As propriedades abaixo serão preenchidas quando for feita a leitura do arquivo retorno recebido do banco e conterão informações fornecidas pelo banco}
ValorDespesaCobranca, {Valor que o banco cobrou pelo serviço de cobrança}
ValorAbatimento, {Valor do abatimento concedido ao sacado}
ValorDesconto, {Valor do desconto concedido ao sacado}
ValorMoraJuros, {Valor dos juros / multa cobrados do sacado pelo banco}
ValorIOF, {Valor do Imposto sobre Operações Financeiras}
ValorOutrasDespesas, {Valor de outras despesas cobradas pelo banco: protesto de títulos, por exemplo}
ValorOutrosCreditos : Currency; {Outros acréscimos informados pelo banco e que serão incluídos no valor repassado ao cedente}

4.1.2 Métodos

Create(AOwner: TComponent); {Cria uma nova instância do componente}
Destroy; {Destrói uma instância do componente}
Assign(ATitulo: TgbTitulo); {Atribui os valores de um componente TgbTitulo a outro componente TgbTitulo. Ex.: gbTitulo2.Assign(gbTitulo1);
EnviarPorEMail(Host, LoginUsuario : string; Porta :integer; Assunto : string; Mensagem : TStringList); {Envia imagem do boleto como anexo de e-mail.}
> Host = número ou nome do servidor de e-mail. Ex.: '200.225.175.1' ou 'www.xyz.com.br'.
> LoginUsuario = Login do usuário no servidor de e-mail. Ex.: 'joao';
> Porta = Porta do servidor utilizada para e-mail. Ex.: 25
> Assunto = Título da mensagem. Ex.: 'Envio de boleto bancário';
> Mensagem = Corpo da mensagem. Ex.: 'Estamos enviando anexo a cobrança referente à primeira parcela da assinatura da revista XYZ';
Visualizar; {Mostra na tela o boleto e permite imprimi-lo}
Imprimir; {Imprime o boleto}


4.2 TGBCOBRANCA

Representa um conjunto de títulos que serão tratados juntos em alguma rotina. Por exemplo: processamento de arquivo retorno e geração de arquivo remessa

4.2.1 Propriedades

NomeArquivo : string; {Nome do arquivo remessa ou retorno}
NumeroArquivo : integer; {Número seqüencial do arquivo remessa ou retorno}
DataArquivo : TDateTime; {Data da geração do arquivo remessa ou retorno}
LayoutArquivo : TLayoutArquivo; {Layout dos arquivos remessa/retorno: CNAB240, CNAB400...}
TipoMovimento : TTipoMovimento; {Tipo de movimento desejado: Remessa, Retorno ...};
Titulos : TgbTituloList; {Títulos incluídos no arquivo remessa ou retorno}
Relatorio : TStringList; {Relatório com os dados contidos no arquivo retorno, porém com formato mais apropriado para a leitura.}

4.2.2 Métodos

Create(AOwner: TComponent); override;
Destroy; override;
LerRetorno : boolean;
GerarRemessa : boolean;

###  ###

5 CLASSES AUXILIARES

5.1 TGBCOBCODBAR

Classe para gerar código de barras para boletos.

5.1.1 Propriedades

Codigo, {Dígitos contidos no código de barras}
LinhaDigitavel : string; {Dígitos da linha digitável. Usada quando não se consegue ler o código de barras}
Imagem    : TImage; {Imagem contendo o código de barras propriamente dito}

5.1.2 Métodos

Create;
Destroy;

5.2 TGBENDERECO

Representa o endereço de cedentes ou sacados

5.2.1 Propriedades

Rua,
Numero,
Complemento,
Bairro,
Cidade,
Estado,
CEP,
EMail : string;

5.2.2 Métodos

Assign(AEndereco: TgbEndereco); {Atribui um objeto TgbEndereço a outro. Ex.: Endereco2.Assign(Endereco1);

5.3 TGBBANCO

Informações sobre o banco

5.3.1 Propriedades

Codigo, {Código do banco na câmara de compensação. Ex.: '341'}
Digito, {Dígito do código do banco. Ex.: '7', no caso do banco '341-7'}
Nome   : string; {Nome do banco. Ex.: 'Banco Itaú S.A'}

5.3.2 Métodos

Assign(ABanco: TgbBanco); {Atribui um objeto TgbBanco a outro. Ex.: gbBanco2.Assign(gbBanco1);

5.4 TGBCONTABANCARIA

Dados da conta bancária de cedentes ou sacados

5.4.1 Propriedades

Banco : TgbBanco; {Banco onde a pessoa tem conta}
CodigoAgencia, {Código da agência}
DigitoAgencia, {Dígito verificador do número da agência}
NumeroConta,   {Número da conta}
DigitoConta,   {Dígito verificador do número da conta}
NomeCliente : string; {Nome do cliente titular da conta}

5.4.2 Métodos

Create; {Cria uma nova instância de objeto TgbContaBancaria}
Destroy; {Destrói uma instância de TgbContaBancaria}
Assign(AContaBancaria: TgbContaBancaria); {Atribui um objeto TgbContaBancaria a outro. Ex.: gbContaBancaria2.Assign(gbContaBancaria1);

5.5 TGBPESSOA

Dados sobre os cedentes ou sacados

5.5.1 Propriedades

TipoInscricao, {'01' - CPF / '02' - CNPJ}
NumeroCPFCGC ,
Nome         : string;
Endereco     : TgbEndereco;
ContaBancaria: TgbContaBancaria;

5.5.2 Métodos

Create;
Destroy;
Assign(APessoa: TgbPessoa);

5.6 TGBCEDENTE

Dados completos sobre o cedente - Classe derivada de TgbPessoa. Portanto, tem todos os métodos e propriedades de TgbPessoa, além dos listados abaixo.

5.6.1 Propriedades

CodigoCedente, {Código que identifica o cedente junto ao banco. Em muitos casos, é igual o número da conta, mas nem sempre.}
DigitoCodigoCedente : string;

5.6.2 Métodos

Assign(ACedente: TgbCedente);


5.7 TGBTITULOLIST

Coleção de objetos do tipo TgbTitulo - Classe descendente de TOjbectList

5.7.1 Propriedades

Items[: integer](Index.md) : TgbTitulo; {Coleção de objetos do tipo TgbTitulo}

5.7.2 Métodos

Create;
Add(ATitulo : TgbTitulo) : integer; {Insere um título no final e retorna o índice relativo à posição dele na coleção}
Remove(ATitulo : TgbTitulo): Integer; {Remove o título ATitulo da coleção}
IndexOf(ATitulo : TgbTitulo): Integer; {Retorna o índice do título ATitulo na coleção}
FindInstanceOf(AClass: TClass; AExact: Boolean = True; AStartAt: Integer = 0): Integer;
Insert(Index: Integer; ATitulo : TgbTitulo); {Insere o título ATitulo na posição especificada por Index}

###  ###

6. COMO INCLUIR NOVOS BANCOS
As rotinas específicas de cada banco, estão incluídas em units chamadas GBCOBXXX.PAS, onde XXX é o código do banco. Veja alguns exemplos de nomes de units de bancos:
GBCOB001.PAS - Banco 001 - Banco do Brasil
GBCOB237.PAS - Banco 237 - Bradesco
GBCOB341.PAS - Banco 341 - Banco Itaú

Para incluir um novo banco, faça o seguinte:
6.1 Abra o pacote Delphi-boleto (arquivo GBCOB.BPR);
6.2 Copie uma das units de bancos existentes e salve com o nome apropriado para o novo banco (GBCOBXXX.PAS, onde XXX é o código do novo banco). Use como modelo o arquivo GBCOB104.PAS (referente à Caixa Econômica Federal).
6.3 Faça as alterações necessárias na unit, seguindo a documentação fornecida por cada banco (rotinas para calcular dígito do nosso número, layout do código de barras, layouts dos arquivos remessa e retorno, etc...). Você pode conseguir alguns desses documentos no site www.gbimoveis.com/genilton. Caso precise de alguma documentação que não se encontre no site, entre em contato com o banco em questão.
6.4 Abra o arquivo GBCOBRANCA.PAS, localize a seção IMPLEMENTATION e inclua o nome da unit do novo banco na cláusula USES da seção IMPLEMENTATION. Vai ficar assim:
**Unit gbCobranca.pas
INTERFACE
...
USES...
...
IMPLEMENTATION
...
USES gbCob001, gbCob021, gbCob104...
...**
6.5 Compile o pacote Delphi-Boleto e corrija os erros detectados;
6.6 Faça cada um dos testes na ordem a seguir e, se detectar erros, corrija-os antes de passar para o próximo teste:
6.6.1 Imprima alguns boletos bancários para o novo banco e faça uma análise visual para ver se parecem estar corretos. Se possível, compare com outros boletos que você tenha em mãos.
6.6.2 Teste a leitura do código de barras em um caixa eletrônico ou peça ao gerente do seu banco para testar no caixa. Lembre-se de cancelar a operação antes de terminar.
6.6.4 Entregue os boletos ao gerente do banco para que ele envie ao departamento responsável para testes completos. Eles testarão diversas coisas como leitura do código de barras, os dados do cedente, linha digitável, etc...
6.6.5 Peça para o banco gerar um arquivo retorno referente aos títulos testados acima. Quando receber o arquivo retorno, teste a leitura dele para ver se está tudo certo. Talvez a forma mais simples de fazer isso seja usar a propriedade Relatório, do componente TgbCobranca. Ela retorna uma lista de strings, apresentando o conteúdo do arquivo retorno em um formato mais agradável para a leitura humana. Se você quiser, pode salvar o conteúdo com a seguinte seqüência de comandos:

gbCobranca1.LerRetorno(NomeArquivoRetorno); {NomeArquivoRetorno é o nome completo do arquivo de retorno que deve ser analisado. Ex: 'C:\COBRANCA\RETORNO.TXT')
gbCobranca1.Relatorio.SaveToFile(NomeArquivoRelatorio); {NomeArquivoRelatorio é o nome completo do arquivo onde será gravado o texto do relatório. Ex.: 'C:\COBRANCA\RELATORIO.TXT');

6.6.6 Gere um arquivo remessa e envie ao banco para testes. Lembre-se de avisar ao banco que é apenas para teste. Se você não avisar, talvez eles processem os títulos, pensando que eles são pra valer. Dependendo do tipo de cobrança que você vai usar, talvez não seja necessário gerar arquivo remessa. Por exemplo, se você vai usar cobrança simples, sem registro (os títulos não ficam registrados no banco) e você mesmo vai imprimir os boletos, você não precisará gerar arquivo remessa.

###  ###

7. TAREFAS QUE AINDA PRECISAM SER INICIADAS, MELHORADAS OU COMPLETADAS

A - Programa DEMO. Peguei um exemplo de programa de contas a receber que tinha desenvolvido para ilustrar o uso de TUPDATESQL e TACTION e aproveitei para a demonstração dos componentes. Talvez esse não seja o programa demo mais apropriado. O ideal seria um programa usando TTABLE e que não tivesse nenhuma detalhe de programação que muitos programadores não estão acostumados a usar.

B - Talvez fosse conveniente não usar componentes de terceiros como o QuickReport e NMSMTP. Esses componentes estão sendo usados para imprimir o boleto e para enviá-lo por e-mail. Isso pode algum problema com versões diferentes do Delphi (que tenham versões diferentes dos componentes ou que não tenham esses componentes)

C - Qualidade da imagem gerada. As propriedades que exibem imagens do boleto (TgbTitulo.Imagem e TgbTitulo.ImagemFichaCompensacao) usam a função GetPage() do QuickReport. Porém, a qualidade da imagem é visivelmente inferior àquela mostrada na tela. Eu imprimi alguns boletos usando TgbTitulo.Imprimir (usa QuickReport.Print), testei no banco Itaú e a leitura do código de barras foi aprovada. Eu gerei a imagem dos mesmos boletos usando TgbTitulo.Imagem (usa a função Getpage do QuickReport), imprimi a imagem, qualidade ficou visivelmente inferior (parece problema de resolução), testei no Banco Itaú e NÃO CONSEGUIU ler o código de barras. A imagem do boleto é usada no método TgbTitulo.EnviarBoletoPorEmail e também na propriedade TgbTitulo.ImagemFichaCompensacao.
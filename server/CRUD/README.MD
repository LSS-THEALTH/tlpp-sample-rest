# tlpp-sample-rest

Exemplos para prover um CRUD atrav�s do Webservice REST TL++

**Resumo**

Para entender o funcionamento de todas as op��es, iremos utilizar um exemplo muito simples de cadastro de Usu�rios de um Sistema.

Com isso, para facilitar o entendimento, criamos um roteiro a seguir e assim, passo a passo, executaremos todas as op��es desenvolvidas nesse projeto de exemplo.

- - - 
***Roteiro de teste***

**1 - Criar base de dados**

Para todos os exemplos, precisaremos de uma Base de Dados e para facilitar a infraestrutura utilizada, faremos uso do SQLite para que n�o seja necess�rio baixar e configurar o TOTVS Dbaccess e instalar nenhum BD.

Portanto, teremos que seguir os passos abaixo:

    a - Verificar na pasta ROOT que n�o tenha nenhuma base SQLite existente, se houver, delete.
	b - Subir appserver
	c - Executar a rotina -> sampleCRUDtlppRestgenerateTables

**2 - Cria usu�rio (PUT)**

Escolha uma das op��es abaixo:

	a - http://127.0.0.1:9080/sample/crud/put/body/user
		com
		{"user":"ana","password":"senha@123","nick":"batatinha","name":"Ana Camargo"}

    b - http://127.0.0.1:9080/sample/crud/put/query/user?user=ana&password=senha@123&name=Ana%20Camargo&nick=batatinha

    c - http://127.0.0.1:9080/sample/crud/put/path/user/ana/senha@123/Ana%20Camargo/batatinha

**3 - Consulta (GET)**

Escolha uma das op��es abaixo:

	a - http://127.0.0.1:9080/sample/crud/get/query/user?user=ana

	b - http://127.0.0.1:9080/sample/crud/get/path/user/ana

**4 - Altera usu�rio (POST)**

Escolha uma das op��es abaixo:

	a - http://127.0.0.1:9080/sample/crud/post/body/user
        com
        {"user":"ana","nick":"aninha","name":"Ana Camargo"}

	b - http://127.0.0.1:9080/sample/crud/post/query/user?user=ana&nick=aninha&name=Ana%20Ferreira

    c - http://127.0.0.1:9080/sample/crud/post/path/user/ana/Ana%20Julia/aninha

**5 - Altera usu�rio (PATCH)**

Escolha uma das op��es abaixo:

	a - http://127.0.0.1:9080/sample/crud/patch/body/user
        com
        {"user":"jose","nick":"zezinho","name":"Josival"}

	b - http://127.0.0.1:9080/sample/crud/patch/path/user/jose/Josival/zezinho

**6 - Exclui usu�rio (DELETE)**

Escolha uma das op��es abaixo:

	a - http://127.0.0.1:9080/sample/crud/delete/body/user
		com
		{"user": "ana"}

    b - http://127.0.0.1:9080/sample/crud/delete/query/user?user=ana

    c - http://127.0.0.1:9080/sample/crud/delete/path/user/ana

**7 - B�nus - valida��o de usu�rio e senha**

	http://127.0.0.1:9080/sample/crud/user/valid
	{"user":"jose","password":"senha@123"}

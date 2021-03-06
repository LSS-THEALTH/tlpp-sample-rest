# Como migrar um REST em FWREST para uso com Annotation

Nesse projeto [ **migrate-FWrest-2-tlpp** ] iremos exemplificar atrav�s de c�digos fontes em ADVPL e TL++ as duas formas de criar servi�os REST.

A primeiro forma ser� como se cria um servi�o REST utilizando o framework Protheus em ADVPL e a segunda como se cria o mesmo servi�o REST, por�m agora utilizando o recurso de Annotation em TL++.

Sendo assim, teremos 3 (tr�s) c�digos fontes, sendo:

Pasta  | Nome arquivo | Linguagem | Tecnologia | Estrutura
:-- | :--| :-- | :-- | :--
src\ | _rest-mod01.prw_  | ADVPL | framework Protheus | classe pr�pria para REST
src\ | _rest-mod02.tlpp_ | TL++ | annotation | user Function
src\ | _rest-mod03.tlpp_ | TL++ | annotation | classe gen�rica


---
## Caracter�sticas

##### Depend�ncias t�cnicas

* **FWREST**

    1. Bin�rio Lobo-Guar�
    2. Protheus
    3. Framework Protheus
    4. License Protheus
    5. Conhecimento de Classe

* **Annotation**

    1. Bin�rio Lobo-Guar�
    2. Core TL++
    3. Conhecimento de annotation


##### Verbos HTTP suportados

* **FWREST**

    - [x] GET
    - [x] POST
    - [x] PUT
    - [ ] PATCH
    - [x] DELETE

* **Annotation**

    - [x] GET
    - [x] POST
    - [x] PUT
    - [x] PATCH
    - [x] DELETE


---
## Resumo das mudan�as

#### Cabe�alho arquivo fonte

* **FWREST**

    ```vb
    #include 'totvs.ch'
    #include "restful.ch"
    ```

* **Annotation**

    ```vb
    #include "tlpp-core.th"
    #include "tlpp-rest.th"
    ```

#### Defini��o do Verbo HTTP

* **FWREST**

    Para utilizar o WSMETHOD � necess�rio criar antes WSRESTFUL, tal como � uma declara��o de classes, por�m com sintaxe espec�fica para o REST.

    ```vb
    WSMETHOD GET
    ```

* **Annotation**

    Nesse caso n�o h� depend�ncia de nenhuma classe e � poss�vel utilizar em fun��es de usu�rios, fun��es e m�todos de classes, ficando � gosto do desenvolvedor.

    ```vb
    @Get()
    ```

#### EndPoint

* **FWREST**

    A composi��o para a defini��o de endpoints nessa vers�o � composta em dois elementos no fonte.

    O primeiro � propriamente o nome da classe te Restful, sendo:

    ```vb
    WSRESTFUL sampleMigrateRestProtheus
    ```

    A outra parte � complementada atrav�s da defini��o da classe, sendo:

    ```vb
    WSMETHOD POST  ....... WSSYNTAX "/sampleMigrateRestProtheus/{id}"
    ```

    Podendo ter altera��es pelo comando PATH, ex:

    ```vb
    WSMETHOD POST ........ PATH  "/v2/sampleMigrateRestProtheus/{id}"
    ```

* **Annotation**

    Por esse modo, voc� pode criar o endpoint sem qualquer rela��o ao nome da classe ou fun��o que ser� executada.

    A defini��o do endpoint � atrav�s da propriedade "endpoint" da annotation, por�m, como ela � a primeira propriedade, pode-se simplesmente colocar seu valor sem nomear a propridade.

    O exemplo abaixo � a forma simples de definir um endpoint:

    ```vb
    @Get("/sampleMigrateRestTlpp")
    ```

    Pode-se tamb�m, definir os path params como segue abaixo:

    ```vb
    @Get("/sampleMigrateRestTlpp/:id")
    ```

    Tamb�m � poss�vel definir uma descri��o para o endpoint, ex:

    ```vb
    @Get(endpoint="/sampleMigrateRestTlpp",description="descri��o do m�todo Get")
    ```


#### Par�metros [ Path Param ]

Essa � forma de passar valores atrav�s de URI como complementos dos paths, ex:
http://localhost:8080/nomerest/cliente/:2345

No exemplo acima, 2345 � o path param para o ID do cliente.

* **FWREST**

    Esse modo � poss�vel resgatar os valores de path param atrav�s da propriedade (array) ::aURLParms, conforme exemplo abaixo:

    ```vb
    if len(::aURLParms) > 0
        cId := ::aURLParms[1]
    endif
    ```

    Por�m, n�o � poss�vel buscar nominalmente o valor, e somente atrav�s da ordem que ele � declarado na URI. <br /><br />

* **Annotation**

    O objeto oRest possui um m�todo espec�fico para retornar somente os par�metros recebidos via Path Param, sendo:

    ```vb
    jPath := oRest:getPathParamsRequest()
    ```

    O retorno do m�todo � uma estrutura em Json com todos os par�metros recebidos via path param, sendo assim, � poss�vel resgatar o valor do par�metro acessando seu valor diretamente no Json atrav�s do seu nome.

    Veja abaixo:

    ```vb
    cId := jPath[ 'id' ]
    ```

#### Par�metros [ Query String ]

Essa � o modo de passar valores atrav�s de URL ap�s todos os paths e de forma nomeada, ex:
http://localhost:8080/nomerest/cliente?id=2345

No exemplo acima, o par�metro leva o nome de "id" e tem o valor de 2345.

* **FWREST**

    Para que possa receber par�metros via query string, � preciso declar�-lo como um par�metro que o m�todo ir� receber, como descrito abaixo:

    ```vb
    WSRESTFUL sampleMigrateRestProtheus ......
        WSDATA id       AS CHARACTER OPTIONAL
    .......

    WSMETHOD GET WSRECEIVE id .....
    ```

    Ap�s isso, pode-se utiliz�-lo como abaixo:
    ```vb
    // Para que n�o seja NIL
    DEFAULT ::id := '' 
    
    // Acessa-se diretamente
    conout( ::id )
    ```

* **Annotation**

    O objeto oRest possui um m�todo espec�fico para retornar somente os par�metros recebidos via query string, sendo:

    ```vb
    jPath := oRest:getQueryRequest()
    ```

    O retorno do m�todo � uma estrutura em Json com todos os par�metros recebidos via query string, sendo assim, � poss�vel resgatar o valor do par�metro acessando seu valor diretamente no Json atrav�s do seu nome.

    Veja abaixo:

    ```vb
    cId := jPath[ 'id' ]
    ```


#### Par�metros [ Body HTTP ]

* **FWREST**

    ```vb
    cBody := ::GetContent()
    ```

* **Annotation**

    ```vb
    cBody := oRest:GetBodyRequest()
    ```

#### Return OK

* **FWREST**

    ```vb
    ::SetResponse( cResponse )
    ```

* **Annotation**

    ```vb
    oRest:setResponse( cResponse )
    ```

#### Return Error

* **FWREST**

    ```vb
    SetRestFault( nCodeHttp, cMessage )
    ```

* **Annotation**

    ```vb
    oRest:setFault( cResponse )
    ```

    Nesse caso o c�digo HTTP do retorno ser� 500, caso tenha a necessidade de trocar o c�digo HTTP, utilize antes o m�todo conforme abaixo:

    ```vb
    oRest:setStatusCode( nCodeHttp )
    oRest:setFault( cResponse )
    ```

---
## Exemplos

> � importante ressaltar que para simplificar este manual iremos unificar os verbos HTTP "PUT", "POST" e "PATCH", pois possuem funcionalidades similares no uso em APIs em REST. Por�m nos arquivos acima descritos todos os exemplos estar�o presentes.

#### Exemplo GET

* **FWREST**

    ```vb
    WSRESTFUL sampleMigrateRestProtheus DESCRIPTION "Exemplo de REST framework Protheus"

        WSDATA id       AS CHARACTER OPTIONAL

        WSMETHOD GET    DESCRIPTION "M�todo Get"    WSSYNTAX "/sampleMigrateRestProtheus"

    END WSRESTFUL


    WSMETHOD GET WSRECEIVE id WSSERVICE sampleMigrateRestProtheus

        local cData := ''

        ::SetContentType("application/json")

        DEFAULT ::id := ''

        cData := '{ "METHOD" : "GET" ,"id" : "' + ::id + '" }'

        ::SetResponse( cData )
    Return .T.
    ```

* **Annotation**

    ```vb
    @Get("/sampleMigrateRestTlpp")
    user function GETsampleMigrateRestTlpp()

        local cId   := ''
        local cData := ''
        local jPath

        jPath := oRest:getQueryRequest()
        if ( jPath <> Nil )
            cId := jPath[ 'id' ]
            if ( valtype(cId) == 'U' )
                cId := ''
            endif
        endif

        cData := '{ "METHOD" : "GET" ,"id" : "' + cId + '" }'

    return oRest:setResponse( cData )
    ```

#### Exemplo POST

* **FWREST**

    ```vb
    WSRESTFUL sampleMigrateRestProtheus DESCRIPTION "Exemplo de REST framework Protheus"
        WSMETHOD POST   DESCRIPTION "M�todo Post"   WSSYNTAX "/sampleMigrateRestProtheus/{id}"
    END WSRESTFUL

    WSMETHOD POST WSSERVICE sampleMigrateRestProtheus

        local cId   := ''
        local cBody := ''
        local jBody

        ::SetContentType("application/json")

        if len(::aURLParms) > 0
            cId := ::aURLParms[1]
        endif

        ::SetResponse( '{ "METHOD" : "POST" ,"id" : "' + cId + '" ' )

        cBody := ::GetContent()
        jBody := JsonObject():new()
        jBody:fromJson( cBody )
        if ( jBody <> Nil )
            if ( !empty(jBody:GetJsonText("cpo1")) )
                ::SetResponse( ', "cpo1" : "' + jBody:GetJsonText("cpo1") + '" ' )
            endif
            if ( !empty(jBody:GetJsonText("cpo2")) )
                ::SetResponse( ', "cpo2" : "' + jBody:GetJsonText("cpo2") + '" ' )
            endif
        endif

        ::SetResponse( ' }' )
    Return .T.
    ```

* **Annotation**

    ```vb
    @Post("/sampleMigrateRestTlpp/:id")
    user function PostsampleMigrateRestTlpp()

        local cId   := ''
        local cData := ''
        local jPath
        local jBody

        jPath := oRest:getPathParamsRequest()
        if ( jPath <> Nil )
            cId := jPath[ 'id' ]
            if ( valtype(cId) == 'U' )
                cId := ''
            endif
        endif

        cData := '{ "METHOD" : "POST" , "id" : "' + cId + '" '

        jBody := JsonObject():new()
        jBody:fromJson( oRest:GetBodyRequest() )
        if ( jBody <> Nil )
            if ( !empty(jBody:GetJsonText("cpo1")) )
                cData += ', "cpo1" : "' + jBody:GetJsonText("cpo1") + '" '
            endif
            if ( !empty(jBody:GetJsonText("cpo2")) )
                cData += ', "cpo2" : "' + jBody:GetJsonText("cpo2") + '" '
            endif
        endif

        cData += '}'

    return oRest:setResponse( cData )
    ```

#### Exemplo DELETE

* **FWREST**

    ```vb
    WSRESTFUL sampleMigrateRestProtheus DESCRIPTION "Exemplo de REST framework Protheus"
        WSMETHOD DELETE DESCRIPTION "M�todo Delete" WSSYNTAX "/sampleMigrateRestProtheus/{id}"
    END WSRESTFUL

    WSMETHOD DELETE WSSERVICE sampleMigrateRestProtheus

        local cId   := ''
        local cData := ''

        ::SetContentType("application/json")

        if len(::aURLParms) > 0
            cId := ::aURLParms[1]
        endif

        cData := '{ "METHOD" : "DELETE" ,"id" : "' + cId + '" }'

        ::SetResponse( cData )
    Return .T.
    ```

* **Annotation**

    ```vb
    @Delete("/sampleMigrateRestTlpp/:id")
    user function DeletesampleMigrateRestTlpp()

        local cId   := ''
        local cData := ''
        local jPath

        jPath := oRest:getPathParamsRequest()
        if ( jPath <> Nil )
            cId := jPath[ 'id' ]
            if ( valtype(cId) == 'U' )
                cId := ''
            endif
        endif

        cData := '{ "METHOD" : "DELETE" , "id" : "' + cId + '" ' + '}'

    return oRest:setResponse( cData )
    ```
---
title: Documentação Api - Sms Empresa

language_tabs:
  - php: PHP
  
  - shell: cURL
  
toc_footers:
  - <a href='http://www.smsempresa.com.br/Criar-Conta' target='_blank'>Criar Conta para obter Chave Key</a>

includes:
  - errors

search: true
---

# Introdução

Bem-vindo a Documentação API da Sms Empresa! Você pode usar no API para acessar os serviços Sms Longo, Sms Curto e Torpedo de Voz.

A comunicação é feita através de protocolo HTTP. Nós temos exemplos de código em PHP e JSON. Você pode ver exemplos de código na area escura a direita, e pode alterar as linguagens clicando na guia superior direita no nome da linguagem.

Qualquer dúvida na utilização ou recursos dessa documentação entre em contato através do e-mail suporte@smsempresa.com.br .

# Sms Longo/Sms Curto

## Envio Simples

```php
<?php
	$ch = curl_init();
	
	$data = array('key'         => 'SUA_CHAVE_KEY', 
				  'type' 		=> '0',
				  'number' 	    => '11988887777',
				  'msg' 		=> 'Teste de envio.',
				  'jobdate' 	=> '01/01/2016',
				  'jobtime' 	=> '10:30',
				  'refer'	    => 'ABC123');
	
	curl_setopt($ch, CURLOPT_URL, 'http://api.smsempresa.com.br/send');
	curl_setopt($ch, CURLOPT_POST, true);
	curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	
	$res    = curl_exec ($ch);
	$err    = curl_errno($ch);
	$errmsg = curl_error($ch);
	$header = curl_getinfo($ch);
	
	curl_close($ch);
	
	print_r($res); 
?>	
```

```shell
curl \
-X POST "http://api.smsempresa.com.br/send" \
-d "key=SUA_CHAVE_KEY \
   &type=0 \
   &number=11988887777 \
   &msg=Teste de envio. \
   &jobdate=01/01/2016 \
   &jobtime=10:30 \
   &refer=ABC123"
```

> O comando acima retorna estrutura XML como esta:

```xml
<?xml version="1.0"?>
<smsempresa>
  <retorno situacao="OK" codigo="1" id="175570049">MENSAGEM NA FILA.</retorno>
</smsempresa>
```

Esse método é utilizado para enviar uma única mensagem por requisição. 

No retorno já é disponibilizado o ID único da mensagem na Sms Empresa. 

Ideal quando as mensagens são diferentes para cada destinatário.

### HTTP Request

`GET or POST http://api.smsempresa.com.br/send`

### Parâmetros

Nome | Tipo | Padrão | Obrigatório | Descrição
--------- | ------- | ----------- | ------ | -------
key | varchar(16) |  | true | Chave de identificação do usuário.
type | number |  | true | Tipo de serviço: 1-Torpedo de Voz.
number | number(13) |  | true | Número destinatário. Não é necessário inserir código do país '55'.
msg	| varchar |  | true | Mensagem de texto.
jobdate | varchar | data atual | false | Data de agendamento para envio. '01/01/2016'.
jobtime | varchar | hora atual | false | Hora de agendamento para envio. '10:30'.
refer | varchar(100) |  | false | Referência do usuário para identificação da mensagem.

## Envio Múltiplo

```php
<?php
	$ch = curl_init();
	
	$data = array('key'         => 'SUA_CHAVE_KEY', 
				  'type' 		=> '0',
				  'number' 	    => '11988887777;21977776666;6288887777',
				  'msg' 		=> 'Teste de envio.',
				  'jobdate' 	=> '01/01/2016',
				  'jobtime' 	=> '10:30',
				  'refer'	    => 'ABC123');
	
	curl_setopt($ch, CURLOPT_URL, 'http://api.smsempresa.com.br/multiple');
	curl_setopt($ch, CURLOPT_POST, true);
	curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	
	$res    = curl_exec ($ch);
	$err    = curl_errno($ch);
	$errmsg = curl_error($ch);
	$header = curl_getinfo($ch);
	
	curl_close($ch);
	
	print_r($res); 
?>	
```

```shell
curl \
-X POST "http://api.smsempresa.com.br/multiple" \
-d "key=SUA_CHAVE_KEY \
   &type=0 \
   &number=11988887777;21977776666;6288887777 \
   &msg=Teste de envio. \
   &jobdate=01/01/2016 \
   &jobtime=10:30 \
   &refer=ABC123"
```

> O comando acima retorna estrutura XML como esta:

```xml
<?xml version="1.0"?>
<smsempres>
  <retorno situacao="OK">MENSAGEM NA FILA.</retorno>
</smsempresa>
```

Esse método é utilizado para enviar múltiplas mensagens com o mesmo texto e destinatários diferentes. 

Você pode incluir até 300 destinatários em uma mesma requisição.

Ideal quando é utilizada a mesma mensagem para vários destinatários.

### HTTP Request

`GET or POST http://api.smsempresa.com.br/multiple`

### Parâmetros

Nome | Tipo | Padrão | Obrigatório | Descrição
--------- | ------- | ----------- | ------ | -------
key | varchar(16) |  | true | Chave de identificação do usuário.
type | number |  | true | Tipo de serviço: 1-Torpedo de Voz.
number | varchar) |  | true | Número destinatário concatenado por ';'. 
msg	| varchar |  | true | Mensagem de texto.
jobdate | varchar | data atual | false | Data de agendamento para envio. '01/01/2016'.
jobtime | varchar | hora atual | false | Hora de agendamento para envio. '10:30'.
refer | varchar(100) |  | false | Referência do usuário para identificação da mensagem.


## Situação

```php
<?php
	$ch = curl_init();
	
	$data = array('key'         => 'SUA_CHAVE_KEY', 
				  'action' 		=> 'status',
				  'id' 	        => '175570049');
	
	curl_setopt($ch, CURLOPT_URL, 'http://api.smsempresa.com.br/get');
	curl_setopt($ch, CURLOPT_POST, true);
	curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	
	$res    = curl_exec ($ch);
	$err    = curl_errno($ch);
	$errmsg = curl_error($ch);
	$header = curl_getinfo($ch);
	
	curl_close($ch);
	
	print_r($res); 
?>	
```

```shell
curl \
-X POST "http://api.smsempresa.com.br/get" \
-d "key=SUA_CHAVE_KEY \
   &action=status \
   &id=175570049"
```

> O comando acima retorna estrutura XML como esta:

```xml
<?xml version="1.0"?>
<smsempresa>
  <retorno situacao="OK" codigo="1" data_envio="20/01/2016 10:33:39" operadora="VIVO-PORTABILIDADE">RECEBIDA</retorno>
</smsempresa>
```

Esse método é utilizado para consultar a situação da mensagem.

Você deve guardar o ID único da mensagem na Sms Empresa no momento do envio para utilizar esse método.

### HTTP Request

`GET or POST http://api.smsempresa.com.br/get`

### Parâmetros

Nome | Tipo | Padrão | Obrigatório | Descrição
--------- | ------- | ----------- | ------ | -------
key | varchar(16) |  | true | Chave de identificação do usuário.
action | varchar |  | true | Definir 'status' para consulta de situação.
id | number |  | true | ID único da mensagem na Sms Empresa.


## Caixa de Entrada

```php
<?php
	$ch = curl_init();
	
	$data = array('key'         => 'SUA_CHAVE_KEY', 
				  'action' 		=> 'inbox',
				  'status'      => '0',
				  'id'          => '175570049');
	
	curl_setopt($ch, CURLOPT_URL, 'http://api.smsempresa.com.br/get');
	curl_setopt($ch, CURLOPT_POST, true);
	curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	
	$res    = curl_exec ($ch);
	$err    = curl_errno($ch);
	$errmsg = curl_error($ch);
	$header = curl_getinfo($ch);
	
	curl_close($ch);
	
	print_r($res); 
?>	
```

```shell
curl \
-X POST "http://api.smsempresa.com.br/get" \
-d "key=SUA_CHAVE_KEY \
   &action=inbox \
   &status=0 \
   &id=175570049"
```

> O comando acima retorna estrutura XML como esta:

```xml
<?xml version="1.0"?>
<smsempresa>
  <retorno situacao="OK" data_read="01/01/2016 11:35:14" telefone="5511988887777" id="" refer_id="" nome="">Resposta 1</retorno>
  <retorno situacao="OK" data_read="01/01/2016 11:36:11" telefone="5521977776666" id="" refer_id="" nome="">Resposta 2</retorno>
  <retorno situacao="OK" data_read="01/01/2016 11:39:24" telefone="556288887777"  id="" refer_id="" nome="">Resposta 3</retorno>
</smsempresa>	
```

Esse método é utilizado para consultar as respostas recebidas.

Respostas estão disponíveis apenas para o serviço 'Sms Curto'.

### HTTP Request

`GET or POST http://api.smsempresa.com.br/get`

### Parâmetros

Nome | Tipo | Padrão | Obrigatório | Descrição
--------- | ------- | ----------- | ------ | -------
key | varchar(16) |  | true | Chave de identificação do usuário.
action | varchar |  | true | Definir 'inbox' para consulta da caixa de entrada.
status | number | 0 | false | Situação da resposta: 0-Somente novas respostas ; 1-Todas as respostas
id | number |  | false | ID único da mensagem na Sms Empresa.

## CallBack

> O método deve estar ser ativadao no menu INBOX -> Configuração CallBack.


Esse método é utilizado para que o servidor da Sms Empresa realize uma chamada via POST ou GET para uma URL definida pelo usuário a cada nova resposta.

É necessário definir a URL no menu INBOX -> Configuração CallBack.

### Parâmetros

Nome | Tipo | Descrição
--------- | ------- | ----------- 
from | number | Remetente da resposta.
message | varchar | Texto da resposta.
id | number | ID único da resposta.
id_sent | number | ID único da mensagem enviada origem da resposta.
refer | varchar | Referência do usuário utilizado na mensagem de origem da resposta.

# Torpedo de Voz

## Envio Simples

```php
<?php
	$ch = curl_init();
	
	$data = array('key'         => 'SUA_CHAVE_KEY', 
				  'type' 		=> '1',
				  'number' 	    => '11988887777',
				  'msg' 		=> 'Teste de envio.',
				  'url_audio'   => 'www.seusiste.com.br/audio.wav',
				  'jobdate' 	=> '01/01/2016',
				  'jobtime' 	=> '10:30',
				  'refer'	    => 'ABC123');
	
	curl_setopt($ch, CURLOPT_URL, 'http://api.smsempresa.com.br/send');
	curl_setopt($ch, CURLOPT_POST, true);
	curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	
	$res    = curl_exec ($ch);
	$err    = curl_errno($ch);
	$errmsg = curl_error($ch);
	$header = curl_getinfo($ch);
	
	curl_close($ch);
	
	print_r($res); 
?>	
```

```shell
curl \
-X POST "http://api.smsempresa.com.br/send" \
-d "key=SUA_CHAVE_KEY \
   &type=1 \
   &number=11988887777 \
   &msg=Teste de envio. \
   &url_audio=www.seusiste.com.br/audio.wav \
   &jobdate=01/01/2016 \
   &jobtime=10:30 \
   &refer=ABC123"
```

> O comando acima retorna estrutura XML como esta:

```xml
<?xml version="1.0"?>
<smsempresa>
  <retorno situacao="OK" codigo="1" id="175570049">MENSAGEM NA FILA.</retorno>
</smsempresa>
```

Esse método é utilizado para enviar uma única mensagem por requisição. 

No retorno já é disponibilizado o ID único da mensgagem na Sms Empresa. 

Ideal quando as mensagens são diferentes para cada destinatário.

### HTTP Request

`GET or POST http://api.smsempresa.com.br/send`

### Parâmetros

Nome | Tipo | Padrão | Obrigatório | Descrição
--------- | ------- | ----------- | ------ | -------
key | varchar(16) |  | true | Chave de identificação do usuário.
type | number |  | true | Tipo de serviço: 0-Sms Longo ; 1-Sms Curto.
number | number(13) |  | true | Número destinatário. Não é necessário inserir código do país '55'.
msg	| varchar |  | false | Mensagem de texto a ser convertida para áudio.
url_audio | varchar |  | false | Url do arquivo de áudio formato '.wav' ou '.mp3'.
jobdate | varchar | data atual | false | Data de agendamento para envio. '01/01/2016'.
jobtime | varchar | hora atual | false | Hora de agendamento para envio. '10:30'.
refer | varchar(100) |  | false | Referência do usuário para identificação da mensagem.


## Envio Múltiplo

```php
<?php
	$ch = curl_init();
	
	$data = array('key'         => 'SUA_CHAVE_KEY', 
				  'type' 		=> '1',
				  'number' 	    => '11988887777;21977776666;6288887777',
				  'msg' 		=> 'Teste de envio.',
				  'jobdate' 	=> '01/01/2016',
				  'jobtime' 	=> '10:30',
				  'refer'	    => 'ABC123');
	
	curl_setopt($ch, CURLOPT_URL, 'http://api.smsempresa.com.br/multiple');
	curl_setopt($ch, CURLOPT_POST, true);
	curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	
	$res    = curl_exec ($ch);
	$err    = curl_errno($ch);
	$errmsg = curl_error($ch);
	$header = curl_getinfo($ch);
	
	curl_close($ch);
	
	print_r($res); 
?>	
```

```shell
curl \
-X POST "http://api.smsempresa.com.br/multiple" \
-d "key=SUA_CHAVE_KEY \
   &type=1 \
   &number=11988887777;21977776666;6288887777 \
   &msg=Teste de envio. \
   &jobdate=01/01/2016 \
   &jobtime=10:30 \
   &refer=ABC123"
```

> O comando acima retorna estrutura XML como esta:

```xml
<?xml version="1.0"?>
<smsempresa>
  <retorno situacao="OK">MENSAGEM NA FILA.</retorno>
</smsempresa>
```

Esse método é utilizado para enviar múltiplas mensagens com o mesmo texto e destinatários diferentes. 

Você pode incluir até 300 destinatários em uma mesma requisição.

Ideal quando é utilizada a mesma mensagem para vários destinatários.

### HTTP Request

`GET or POST http://api.smsempresa.com.br/multiple`

### Parâmetros

Nome | Tipo | Padrão | Obrigatório | Descrição
--------- | ------- | ----------- | ------ | -------
key | varchar(16) |  | true | Chave de identificação do usuário.
type | number |  | true | Tipo de serviço: 0-Sms Longo ; 1-Sms Curto.
number | varchar |  | true | Número destinatário concatenado por ';'. 
msg	| varchar |  | true | Mensagem de texto a ser convertida para áudio.
jobdate | varchar | data atual | false | Data de agendamento para envio. '01/01/2016'.
jobtime | varchar | hora atual | false | Hora de agendamento para envio. '10:30'.
refer | varchar(100) |  | false | Referência do usuário para identificação da mensagem.


## Situação

```php
<?php
	$ch = curl_init();
	
	$data = array('key'         => 'SUA_CHAVE_KEY', 
				  'action' 		=> 'status',
				  'id' 	        => '175570049');
	
	curl_setopt($ch, CURLOPT_URL, 'http://api.smsempresa.com.br/get');
	curl_setopt($ch, CURLOPT_POST, true);
	curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	
	$res    = curl_exec ($ch);
	$err    = curl_errno($ch);
	$errmsg = curl_error($ch);
	$header = curl_getinfo($ch);
	
	curl_close($ch);
	
	print_r($res); 
?>	
```

```shell
curl \
-X POST "http://api.smsempresa.com.br/get" \
-d "key=SUA_CHAVE_KEY \
   &action=status \
   &id=175570049"
```

> O comando acima retorna estrutura XML como esta:

```xml
<?xml version="1.0"?>
<smsempresa>
  <retorno situacao="OK" codigo="1" data_envio="01/01/2016 10:30:00" data_atendeu_voz="01/01/2016 10:31:15" operadora="VIVO-PORTABILIDADE">RECEBIDA</retorno>
</smsempresa>
```

Esse método é utilizado para consultar a situação da mensagem.

Você deve guardar o ID único da mensagem na Sms Empresa no momento do envio para utilizar esse método.

### HTTP Request

`GET or POST http://api.smsempresa.com.br/get`

### Parâmetros

Nome | Tipo | Padrão | Obrigatório | Descrição
--------- | ------- | ----------- | ------ | -------
key | varchar(16) |  | true | Chave de identificação do usuário.
action | varchar |  | true | Definir 'status' para consulta de situação.
id | number |  | true | ID único da mensagem na Sms Empresa.


# Conta

## Saldo

```php
<?php
	$ch = curl_init();
	
	$data = array('key'         => 'SUA_CHAVE_KEY', 
				  'action' 		=> 'saldo');
	
	curl_setopt($ch, CURLOPT_URL, 'http://api.smsempresa.com.br/get');
	curl_setopt($ch, CURLOPT_POST, true);
	curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	
	$res    = curl_exec ($ch);
	$err    = curl_errno($ch);
	$errmsg = curl_error($ch);
	$header = curl_getinfo($ch);
	
	curl_close($ch);
	
	print_r($res); 
?>	
```

```shell
curl \
-X POST "http://api.smsempresa.com.br/get" \
-d "key=SUA_CHAVE_KEY \
   &action=saldo"
```

> O comando acima retorna estrutura XML como esta:

```xml
<?xml version="1.0"?>
<smsempresa>
 <retorno situacao="OK" saldo_longo="100" saldo_curto="22" saldo_voz="12" saldo_email="0">SALDO ATUAL</retorno>
</smsempresa>
```

Esse método é utilizado para consultar saldo de todos os serviços. 

Ideal para que o usuário não fique sem saldo.

### HTTP Request

`GET or POST http://api.smsempresa.com.br/get`

### Parâmetros

Nome | Tipo | Padrão | Obrigatório | Descrição
--------- | ------- | ----------- | ------ | -------
key | varchar(16) |  | true | Chave de identificação do usuário.
action | varchar |  | true | Definir 'saldo' para consulta de saldo.
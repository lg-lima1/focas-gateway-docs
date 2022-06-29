Instalação
==========

Os aplicativos da plataforma ctrlX Automation, podem ser disponibilizados em dois formatos ``.snap`` ou ``.app``. No caso de aplicativos no formato ``.snap``, tem-se dois arquivos distintos de instalação, cada um voltado a uma arquitetura específica, de acordo com a arquitetura do ctrlX Core (``amd64`` ou ``arm64``).

.. _arquiteturas:

Arquiteturas
------------

ctrlX Core Virtual
  O ctrlX Core Virtual é uma máquina virtual que simula todas as funcionalidades de um ctrlX Core na máquina do usuário. Nesse caso, tem-se a arquitetura ``amd64``, portanto o aplicativo a ser instalado deve ter um sufixo em seu nome identificando o suporte a essa arquitetura.

  ``focas-gateway_{versão}_amd64.snap``

ctrlX Core
  O ctrlX Core tem a arquitetura ``arm64``. Dessa forma, o aplicativo a ser instalado deve ter um sufixo em seu nome identificando o suporte a essa arquitetura.

  ``focas-gateway_{versão}_arm64.snap``

.. _instalação:

Procedimento
------------
Para habilitar a instalação de aplicativos, primeiro deve-se alterar para *Service Mode*.

.. image:: imgs/install/service-mode.png
  :width: 600
  :alt: Service Mode

Na sequência deve-se habilitar a instalação de aplicativos de terceiros no ctrlX Core. Para isso, acesse o menu *settings*.

.. image:: imgs/install/goto-settings.png
  :width: 600
  :alt: Go to settings

E clique na opção *Allow installation from unknown source*.

.. image:: imgs/install/allow-third-party-app.png
  :width: 400
  :alt: Allow third-party app

Na sequência, enviar o arquivo ``.snap`` ou ``.app`` referente ao aplicativo a ser instalado. Para arquivos ``.snap``, verificar corretamente a arquitetura suportada. Se o arquivo de instalação foi enviado corretamente, a janela abaixo deve aparecer contendo o nome do aplicativo e a sua versão semântica.

.. image:: imgs/install/install-from-file.png
  :width: 300
  :alt: Install from file

Após a correta instalação do aplicativo, retornar para *Operation Mode*.

.. image:: imgs/install/operation-mode.png
  :width: 600
  :alt: Operation Mode

Parabéns! Você instalou o FOCAS 2 Gateway.

Na próxima seção, vamos configurar o endereço da portas ethernet do ctrlX Core, de modo que seja possível a comunicação com o comando CNC Fanuc. :ref:`configurações`.
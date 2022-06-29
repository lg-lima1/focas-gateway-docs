Aquisição
=========

Essa seção tem como objetivo descrever as diversas informações que podem ser coletadas do comando CNC Fanuc configurado na seção anterior através do ctrlX Datalayer.
A estrutura dessa seção descreve cada dado coletado como um `node` dentro do ctrlX Datalayer. 
Todos os dados requisitados retornam um objeto `JSON` como representação do dado. Mais informações sobre a estrutura retornada estão na descrição de cada node.

.. autosummary::

alarm
-----

O node **focas-gateway > alarm** coleta o estado de todos os bits de alarme do comando NC, assim como a mensagem descritiva dos alarmes ativos.

Esse node somente implementa o método `read`. Quando chamado retornará um objeto `JSON` com a estrutura abaixo.

.. code-block:: json

    {
        "status": {
            "parameter_switch": bool,
            "power_off_parameter_set": bool,
            "io_error": bool,
            "foreground_ps": bool,
            "overtravel": bool,
            "overheating": bool,
            "servo": bool,
            "data_io_error": bool,
            "macro": bool,
            "spindle": bool,
            "other_DS": bool,
            "malfunction_prevent": bool,
            "background_ps": bool,
            "syncronized_error": bool,
            "external": bool,
            "pmc_error": bool
        },
        "messages": array of strings
    }

axis-data
---------

O node **focas-gateway > axis-data** representa o estado atual de eixos de posicionamento do comando NC e contém child nodes (ou, nós filhos) representando cada eixo. A informação da quantidade de eixos, assim como o nome do eixo, é coletado durante a execução do programa de forma dinâmica.

Esse node implementa o método `browse`. Quando chamado retornará uma lista dos child nodes, que representam o nome o nome de cada eixo.

Nesse exemplo, temos um comando NC com três eixos, X, Y e Z, respectivamente. Chamando o método `browse`, nos retornará:

.. code-block:: json

    [
        "X",
        "Y",
        "Z"
    ]

Cada eixo (child node) implementa o método `read`. Dessa forma, quando chamado retornará um objeto `JSON` representando o estado atual do eixo requisitado. A estrutura é idêntica entre todos os eixos.

Nesse exemplo, estamos lendo os dados do eixo X através do node `focas-gateway > axis-data > X`. Chamando o método `read`, nos retornará:

.. code-block:: json

    {
        "position" : [
            {
                "parameter_name": "Absolute position",
                "value": number,
                "unit": string ["mm", "inch"]
            },
            {
                "parameter_name": "Machine position",
                "value": number,
                "unit": string ["mm", "inch"]
            },
            {
                "parameter_name": "Relative position",
                "value": number,
                "unit": string ["mm", "inch"]
            },
            {
                "parameter_name": "Distance to go",
                "value": number,
                "unit": string ["mm", "inch"]
            },
        ],
        "servo" : [
            {
                "parameter_name": "Servo load meter",
                "value": number,
                "unit": "%"
            },
            {
                "parameter_name": "Load current",
                "value": number,
                "unit": "%"
            },
            {
                "parameter_name": "Load current",
                "value": number,
                "unit": "A"
            },
        ]
    }

parameters
----------

O node **focas-gateway > parameters** representa o valor de parâmetros do comando NC e contém child nodes (oun nós filhos) para cada parâmetro. Os parâmetros a serem requisitados são informados pelo usuário através do próprio ctrlX Datalayer.

Esse node implementa o método `create`. Ao executá-lo é preciso que o usuário informe qual o parâmetro a ser lido (em formato numérico) no corpo da requisição, caso contrário, o método retornará erro.

Nesse exemplo vamos cadastrar o parâmetro ``1020`` usando o node **focas-gateway > parameters**. 

.. code-block:: json

    1020

Esse node também implementa o método `browse`. Ao executá-lo, o método retornará uma lista de todos seus child nodes, onde cada child node é uma representação dos parâmetros previamente cadastrados pelo método `create`.

Nesse exemplo, usando o node **focas-gateway > parameters**, vemos que temos cadastrados os parâmetros ``1020`` e ``1320``.

.. code-block:: json

    [
        1020,
        1320
    ]

Cada parâmetro (child node) implementa o método `read`. Dessa forma, quando chamado retornará um objeto `JSON` representando o estado atual do parâmetro requisitado. A quantidade de posições no array varia de acordo com o parâmetro: se ele representa um parâmetro de sistema, deve ser um array de uma posição; se ele representa um parâmetro de eixo, deve ser um array de tamanho igual a quantidade de eixos do sistema.

Nesse exemplo, usando o node **focas-gateway > parameters > 1020**, o método `read` nos retornará:

.. code-block:: json

    {
        "data" : array of numbers
    }

pmc-alarm
---------

O node **focas-gateway > pmc-alarm** representa as mensagens de alarme presentes no comando NC no momento da requisição que foram acionadas durante a execução do programa PMC.

Esse node implementa o método `read`. Ao executá-lo, o método retornará um objeto `JSON` com a estrutura abaixo.

.. code-block:: json

    {
        "messages": array of strings
    }

pmc-data
--------

O node **focas-gateway > pmc-data** representa o valor de endereços de memória do programa PMC do comando NC e contém child nodes (ou nós filhos) para cada endereço. Os endereços a serem requisitados são informados pelo usuário através do próprio ctrlX Datalayer.

Esse node implementa o método `create`. Ao executá-lo é preciso que o usuário informe qual o endereço a ser lido (em formato textual) no corpo da requisição, caso contrário, o método retornará erro. Adicionalmente, o endereço a ser cadastrado deve ser válido, no seguinte formato ``X1234``, onde: X, é um único caractére que representa a categoria do endereço de memória, e posteriormente, quatro dígitos seguidos. Consultar o manual de parâmetros da Fanuc para a família do comando para mais informações.

Nesse exemplo vamos cadastrar o endereço ``C0034`` usando o node **focas-gateway > parameters**. 

.. code-block:: json

    C0034

Esse node também implementa o método `browse`. Ao executá-lo, o método retornará uma lista de todos seus child nodes, onde cada child node é uma representação dos endereços de memória previamente cadastrados pelo método `create`.

Nesse exemplo, usando o node **focas-gateway > pmc-data**, vemos que temos cadastrados os parâmetros ``C0034`` e ``C0126``.

.. code-block:: json

    [
        C0034,
        C0126
    ]

Cada parâmetro (child node) implementa o método `read`. Dessa forma, quando chamado retornará um objeto `JSON` representando o valor atual no endereço de memória requisitado.

Nesse exemplo, usando o node **focas-gateway > pmc-data > C0034**, o método `read` nos retornará:

.. code-block:: json

    {
        "data": number
    }

pmc-title
---------

O node **focas-gateway > pmc-title** retorna informações referentes ao programa PMC carregado no comando NC. Essas informações são inseridas pelo desenvolvedor do programa PMC e estão atreladas ao programa em execução no comando NC.

Esse node implementa o método `read`. Ao executá-lo, o método retornará um objeto `JSON` com a estrutura abaixo.

.. code-block:: json

    {
        "machine_tool_builder_name": string,
        "machine_tool_name": string,
        "type_name": string,
        "progno": string,
        "progvers": string,
        "progdraw": string,
        "date": string,
        "designed_by": string,
        "written_by": string,
        "remarks": string
    }

program-info
------------

O node **focas-gateway > program-info** contém informações referentes ao nome e número do programa em execução, assim como a quantidade de programas registrados e a memória do comando utilizada para armazenamento dos programas.

Esse node implementa o método `read`. Ao executá-lo, o método retornará um objeto `JSON` com a estrutura abaixo.

.. code-block:: json

    {
        "program_name": string,
        "program_no": number,
        "main_prg_no": number,
        "registered_files": number, /* No. programs registered */
        "available_files": number, /* No. programs available */
        "used_memory": number, /* Used memory in 'kb' */
        "available_memory": number /* Available memory in 'kb' */
    }

speed-data
----------

O node **focas-gateway > speed-data** contém informações referentes a velocidade dos eixos de posicionamento e spindles controlados pelo comando NC.

Esse node implementa o método `read`. Ao executá-lo, o método retornará um objeto `JSON` com a estrutura abaixo.

.. code-block:: json

    {
        "speed": [
            {
                "parameter_name": "Feed rate (F)",
                "value": number,
                "unit": string ["mm/minute", "inch/minute"]
            },
            {
                "parameter_name": "Spindle speed (S)",
                "value": number,
                "unit": "rpm"
            },
            {
                "parameter_name": "JOG / Dry Run speed",
                "value": number,
                "unit": string ["mm/minute", "inch/minute"]
            }
        ]
    }

spindle-data
------------

O node **focas-gateway > spindle-data** contém informações referentes ao eixo spindle como, sua carga e sua velocidade, parametrizada e real.

Esse node implementa o método `read`. Ao executá-lo, o método retornará um objeto `JSON` com a estrutura abaixo.

.. code-block:: json

    {
        "spindle": [
            {
                "parameter_name": "Spindle load meter",
                "value": number,
                "unit": "%"
            },
            {
                "parameter_name": "Spindle motor speed",
                "value": number,
                "unit": "rpm"
            },
            {
                "parameter_name": "Spindle speed (3799#2)",
                "value": number,
                "unit": "rpm"
            },
            {
                "parameter_name": "Spindle speed (motor speed)",
                "value": number,
                "unit": "rpm"
            }
        ]
    }

status-info
-----------

O node **focas-gateway > status-info** contém informações referentes aos modos de operação do comando NC, assim como status de alguns módulos de monitoramento como emergência e alarme.

Esse node implementa o método `read`. Ao executá-lo, o método retornará um objeto `JSON` com a estrutura abaixo.

.. code-block:: json

    {
        "hdck": string, /* Status of manual handle re-trace */
        "tmmode": string, /* T/M mode selection */
        "aut": string, /* AUTO/MANUAL mode selection */
        "run": string, /* Status of automatic operation */
        "motion": string, /* Status of axis movement, dwell */
        "mstb": string, /* Status of M, S, T, B funtions */
        "emergency": string, /* Status of emergency */
        "alarm": string, /* Status of alarm */
        "edit": string /* Status of program editing */
    }

system-info
-----------

O node **focas-gateway > system-info** contém informações referentes as características do comando NC, seu modelo, quantidade de eixos e módulos opcionais suportados.

Esse node implementa o método `read`. Ao executá-lo, o método retornará um objeto `JSON` com a estrutura abaixo.

.. code-block:: json

    {
        "loader_control": bool,
        "i_series": bool,
        "compound_machining": bool,
        "transfer_line": bool,
        "plus_type": bool,
        "model_info": string,
        "max_axis": number,
        "cnc_type": string,
        "mt_type": string,
        "series": string,
        "version": string,
        "axes": number
    }

timers
------

O node **focas-gateway > timers** contém informações referentes aos temporizadores internos do comando NC. Com esse node é possível monitorar os tempos de máquina ligada, em operação, em ciclo de corte, entre outros.

A menos que esteja indicado, todos os valores são em segundos e atualizados a cada segundo.

Esse node implementa o método `read`. Ao executá-lo, o método retornará um objeto `JSON` com a estrutura abaixo.

.. code-block:: json

    {
        "powered": number, /* updated every minute */
        "in_operation": number,
        "cutting": number,
        "cycle_time": number,
        "free_time": number
    }

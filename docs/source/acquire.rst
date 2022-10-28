Acquire
=========

This section shows the different types of data that can be acquired from the FANUC controller configured in the last section :ref:`configurations`. Each type of data is described in this section as a `node` inside the ``ctrlX Datalayer``.

All nodes that implement the `read` method return a `JSON` object as a representation of the data. More information about the object strucutre is detailed in each `node` description.

This sections requires a basic understanding on how the ``ctrlX Datalayer`` works and what functionalities it has. Please refer to the `ctrlX Datalayer <https://developer.community.boschrexroth.com/t5/Store-and-How-to/FAQ-for-ctrlX-Data-Layer/ba-p/21236>`_ documentation for more information.

.. autosummary::

alarm
-----

The `node` **focas-gateway > alarm** acquire the alarm status bits from the controller, as well as the alarm description message.

This `node` only implementes the `read` method. When called, it returns a `JSON` object with the following schema:

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

The `node` **focas-gateway > axis-data** represents the actual positioning axis status with each axis currently available in the FANUC controller represented as a `child node`. The currently available axis are loaded dynamically during the execution of the app.

This `node` implements the `browse` method. When called, it will return a list of `child nodes` that represent the name of each axis currently available at the FANUC controller.

As an example, a FANUC controller has three axis, X, Y and Z. Calling `browse` method, will return:

.. code-block:: json

    [
        "X",
        "Y",
        "Z"
    ]

Each axis or `child node` implements the `read` method individually. When called, it returns a `JSON` object representing the actual status of that axis. The same data structure is applied to all axis.

As an example, the X axis status can be acquired through the `node` `focas-gateway > axis-data > X`. Calling `read` method, will return a `JSON` object with the following schema:

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

The `node` **focas-gateway > parameters** represents the FANUC controller parameters with each requested parameter as a `child node`. The requested parameters are to be defined by the user through the ``ctrlX Datalayer`` usign the `create` method.

This `node` implements the `create` method. When calling it, the user must inform the address of the requested parameter as an `integer` in the method itself. Failing to do so will result in an error.

This `node` also implements the `browse` method. When called, it returns a list of your `child nodes`, where each one is a representation of the parameters previously requested usign the `create` method.

As an example, calling `browse` method on `node` **focas-gateway > parameters** returns the parameters ``1020`` and ``1320``.

.. code-block:: json

    [
        1020,
        1320
    ]

Each parameter or `child node` implements the `read` method. When called, it returns a `JSON` object that represents the actual value of the requested parameter. Some parameters represent a single system parameter, while others might represent axis parameters. The size of the returned value array may vary according to the requested parameter to reflect that.

As an example, calling the `read` method on `node` **focas-gateway > parameters > 1020**, will return a `JSON` object with the following schema:

.. code-block:: json

    {
        "data" : array of numbers
    }

pmc-alarm
---------

The `node` **focas-gateway > pmc-alarm** represents alarm messages present at the FANUC controller that were raised by the PMC runtime execution.

This `node` implements the `read` method. When called, it returns a `JSON` object with the following schema:

.. code-block:: json

    {
        "messages": array of strings
    }

pmc-data
--------

The `node` **focas-gateway > pmc-data** represents the requested PMC memory addresses values with each requested PMC memory address as a `child node`. The requested addresses are to be defined by the user through the ``ctrlX Datalayer`` usign the `create` method.

This `node` implements the `create` method. When calling this method, the user must inform the address of the requested PMC memory address as an `string` in the method itself. Failing to do so will result in an error. Additionaly, the requested PMC memory address must be valid and follow the following format ``X1234``,  where *X* is a character that represents the type of memory address, followed by 4 digits. Please refer to your FANUC controller on more information about PMC memory addresses types.

This `node` also implements the `browse` method. When called, it returns a list of `child nodes`, where each one is a representation of the PMC memory addresses previously requested usign the `create` method.

As an example, calling `browse` method on `node` **focas-gateway > pmc-data**, it returns the PMC memory addresses ``C0034`` and ``C0126``. ``C`` stands for counter in the PMC memory address types.

.. code-block:: json

    [
        C0034,
        C0126
    ]

Each PMC memory address or `child node` implements the `read` method. When called, it returns a `JSON` object that represents the actual value of that PMC memory address. As an example, calling the `read` method on `node` **focas-gateway > pmc-data > C0034**, will return a `JSON` object with the following schema:

.. code-block:: json

    {
        "data": number
    }

pmc-title
---------

The `node` **focas-gateway > pmc-title** represents informations about the loaded PMC project on the FANUC controller. These informations are bound by the developer to the PMC project.

This `node` implements the `read` method. When called, it returns a `JSON` object with the following schema:

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

The `node` **focas-gateway > program-info** represents information about the loaded NC program, as well as the available file count and memory size for storage of the NC programs.

This `node` implements the `read` method. When called, it returns a `JSON` object with the following schema:

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

The `node` **focas-gateway > speed-data** represents the speed data from the positioning axis and spindles available at the FANUC controller.

This `node` implements the `read` method. When called, it returns a `JSON` object with the following schema:

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

The `node` **focas-gateway > spindle-data** represents informations about the spindle axis available at the FANUC controller. It shows spindle actual load and actual motor speed.

This `node` implements the `read` method. When called, it returns a `JSON` object with the following schema:

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

The `node` **focas-gateway > status-info** represents informations about the different operation modes of the FANUC controller, as well as status from emergency and alarm monitoring systems.

This `node` implements the `read` method. When called, it returns a `JSON` object with the following schema:

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

The `node` **focas-gateway > system-info** represents informations regarding the characteristics of the FANUC controller - model, axis count, supported axis count and aditional optional modules.

This `node` implements the `read` method. When called, it returns a `JSON` object with the following schema:

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

The `node` **focas-gateway > timers** represents information regarding the internal timers of the FANUC controller. With this node it's possible to monitor controller power on time, in operation time, and others.

Unless noted, all values are in seconds and updated every second.

This `node` implements the `read` method. When called, it returns a `JSON` object with the following schema:

.. code-block:: json

    {
        "powered": number, /* updated every minute */
        "in_operation": number,
        "cutting": number,
        "cycle_time": number,
        "free_time": number
    }

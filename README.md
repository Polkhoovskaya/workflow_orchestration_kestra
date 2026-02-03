## Kestra

### Заполняются при запуске flow
inputs:
  - id: name
    type: STRING
    defaults: Will

### Глобальные или локальные параметры, которые можно использовать во многих tasks
variables:
  welcome_message: "Hello, {{ inputs.name }}!"

### render() — говорит: «воспринимай содержимое этой переменной как шаблон и подставь все значения»

tasks:
  - id: hello_message
    type: io.kestra.plugin.core.log.Log
    message: "{{ render(vars.welcome_message) }}"

### Таски типа output генерируют значения, которые потом можно использовать в других тасках

- id: generate_output
    type: io.kestra.plugin.core.debug.Return
    format: I was generated during this workflow.

- id: log_output
  type: io.kestra.plugin.core.log.Log
  message: "This is an output: {{ outputs.generate_output.value }}"


### pluginDefaults нужен, чтобы задать значения по умолчанию для плагинов во всём flow — чтобы не писать одно и то же в каждом task’е

pluginDefaults:
  - type: io.kestra.plugin.core.log.Log
    values:
      level: ERROR

### Задаёт тип реагирования flow

triggers:
  - id: schedule
    type: io.kestra.plugin.core.trigger.Schedule
    cron: "0 10 * * *"
    inputs:
      name: Sarah
    disabled: true

### concurrency в Kestra управляет сколько экземпляров одного flow может выполняться одновременно
concurrency:
  behavior: FAIL
  limit: 2

# YAML
**YAML (YAML Ain’t Markup Language) - формат для записи данных.**
+ **Формирование** происходит при помощи **пробелов**
+ Знак решетки **`#` обозначает комментарий**, и продолжается до завершегия строки.
+ Чтобы обозначить список, необоходимо перед каждым элементом списка ставить **минус** `-`, **все строки в списке должны быть на одном уровне отступа**:
    ```yaml
    - me
    - you
    - he
    - she
    - it
    ```
    Список можно представить в одну строку, тогда он больше похож на формат JSON:
    ```yaml
    [me, you, he, she, it]
    ```
+ Чтобы обозначить словарь, можно сипользовать двоеточие `:`, **все элементы в словаре должны быть на одном уровне**:
    ```yaml
    me: alex
    you: vladimir
    he: kiril
    she: yana
    it: alina
    ```
    Элементы словаря можно распологать в одну строку:
    ```yaml
    {me: alex, you: vladimir, he: kiril, she: yana, it: alina}
    ```
+ Строки не обязательно брать в ковычки, однако желательно, если вдруг в них применяются специфические для yaml знаки:
    ```yaml
    command: "sh interface | include Queueing strategy:"
    ```
+ Комбинация элементов:
    + Словарь со списками
        ```yaml
        access:
        - switchport mode access
        - switchport access vlan
        - switchport nonegotiate
        - spanning-tree portfast
        - spanning-tree bpduguard enable

        trunk:
        - switchport trunk encapsulation dot1q
        - switchport mode trunk
        - switchport trunk native vlan 999
        - switchport trunk allowed vlan
        ```
    + Список со словарями
        ```yaml
        - BS: 1550
          IT: 791
          id: 11
          name: Liverpool
          to_id: 1
          to_name: LONDON
        - BS: 1510
          IT: 793
          id: 12
          name: Bristol
          to_id: 1
          to_name: LONDON
        - BS: 1650
          IT: 892
          id: 14
          name: Coventry
          to_id: 2
          to_name: Manchester
        ```
+ Существует тип данных:
    ```yaml
    integer: 25
    string: "25"
    float: 25.0
    boolean: true
    null type: null
    ```
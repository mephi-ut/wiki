Обстановка:
С двух внешних номеров одинаковым образом производится перенаправление в exten «pk» (это call-центр). Однако, когда звонят через один — всё хорошо. Когда звонят через другой — очень часто бывают обрывы примерно на 30-ой секунде.

Диагностика долгая и бесполезная. Самое интересное из найденного во всеразличных логах и дампах, это что:
 * при каждом обрыве видно (совпадение?): Packet timed out after 32000ms with no response;
 * само завершение звонка на вид происходит по инициативе телефонного аппарата оператора call-центра (который в Dial плана вызова ниже) с указанием следующией причины: Reason: SIP ;text="Onhook event"

```
MySQL [...]> select * from external where exten='pk';
| id  | context    | exten | priority | app        | appdata
|  34 | ext_menues | pk    |        1 | Answer     |
|  35 | ext_menues | pk    |        2 | Wait       | 1
|  36 | ext_menues | pk    |        3 | Playback   | menu_pk
|  37 | ext_menues | pk    |        4 | Set        | RECFILE=${STRFTIME(,,%Y_%m_%d-%H_%M_%S)}-${CALLERID(num)}
|  38 | ext_menues | pk    |        5 | MixMonitor | /var/spool/asterisk/monitor/pk/${RECFILE}.wav
|  39 | ext_menues | pk    |        6 | Dial       | ...
| 125 | ext_menues | pk    |        7 | GotoIf     | $[${DIALSTATUS}=BUSY]?pk,9
| 126 | ext_menues | pk    |        8 | Dial       | ...
| 127 | ext_menues | pk    |        9 | Hangup     |
```
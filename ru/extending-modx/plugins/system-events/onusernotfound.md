---
title: "OnUserNotFound"
translation: "extending-modx/plugins/system-events/onusernotfound"
---

## Событие: OnUserNotFound

Срабатывает, когда пользователь не найден во время входа в систему.

Может использоваться для обеспечения внешней аутентификации путем возврата массива, где один из индексов в массиве является экземпляром (или расширением) объекта modUser.

Служба: 6 - User Defined Events
Группа: modUser

## Параметры события

| Имя          | Описание                                                                                     |
| ------------ | -------------------------------------------------------------------------------------------- |
| username     | Указанное имя пользователя.                                                                  |
| password     | Указанный пароль.                                                                            |
| attributes   | Массив: - rememberme - Булево множество, если пользователь хочет, чтобы пароль был запомнен. |
| lifetime }   | Срок действия файла cookie сеанса для этого логина.                                          |
| loginContext | Ключ контекста, в котором происходит вход в систему.                                         |
| addContexts  | Дополнительные контексты, в которых вход в систему также происходит.                         |

## Event Login Workflow

1. _[_OnBeforeWebLogin_](extending-modx/plugins/system-events/onbeforeweblogin)_ || _[OnBeforeManagerLogin](extending-modx/plugins/system-events/onbeforemanagerlogin)_ - Внутри этого события разработчик может проверить наличие ошибочных параметров, которые **будут запрещать** дальнейшую регистрацию в процессе. Если плагины, выполненные этим событием, возвращают что-то, кроме true, вход в систему будет прерван с указанной ошибкой.
2. **_OnUserNotFound_** - Это событие выполняется, только если указанное имя пользователя не найдено в базе данных MODX. Разработчик может предоставить свой собственный объект modUser в выходных данных события, чтобы продолжить процесс входа в систему.
3. _[OnWebAuthentication](extending-modx/plugins/system-events/onwebauthentication)_ || _[OnManagerAuthentication](hextending-modx/plugins/system-events/onmanagerauthentication)_ - Внутри этого события разработчик может проверить параметры, которые **отменят проверку по умолчанию паролем** и **позволят** продолжить вход в систему. Если один из плагинов, выполненных из этого события, возвращает true, пользователь считается проверенным и вошел в систему.
4. _[OnWebLogin](extending-modx/plugins/system-events/onweblogin)_ || _[OnManagerLogin](extending-modx/plugins/system-events/onmanagerlogin)_ - Это событие вызывается после завершения процесса входа в систему и считается, что пользователь вошел в систему. Оно **не меняет **процесс входа в систему **поведение**.

## Смотри также

- [System Events](extending-modx/plugins/system-events "System Events")
- [Plugins](extending-modx/plugins "Plugins")

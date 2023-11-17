# Описание OpenVK API

OpenVK API имитирует VKAPI. Если вы хотите улучшить его, [посетите эту страницу](https://github.com/openvk/openvk/blob/master/VKAPI/README.md).

Чтобы вызвать функцию, вам нужно перейти на URL `{ВАШ_ДОМЕН}/method/` и после этого название функции, например: `{ВАШ_ДОМЕН}/method/Account.getProfileInfo`. Сервер вернёт данные в формате JSON. Вы можете использовать GET и POST (для загрузки фотографий на сервер, например) запросы.

🔰 рядом с функцией означает, что она требует авторизацию.

## Авторизация.

Приложения на данный момент не реализованы, и токен можно получить через ваш логин и пароль. Для этого перейдите по этому URL:

`{ВАШ_ДОМЕН}/token?username={ВАШ_Email}&password={ВАШ_ПАРОЛЬ}&grant_type=password`

Вы получите сообщение вроде:

```json
{
    "access_token": "Здесь-будет-токен-действительно-длинный-токен-jill",
    "expires_in": 0,
    "user_id": 1
}
```

Если вы вызываете функцию, требующую токен, задайте параметр `access_token` с токеном.

Если у вас включена двухфакторная аутентификация, добавьте параметр `code` в ваш запрос и заполните его кодом.

## Account

### `getProfileInfo` 🔰

Возвращает информацию о вашем аккаунте.

```json
{
    "response":
    {
        "first_name": "Владимир",
        "id": 1,
        "last_name": "Баринов",
        "home_town": "Москва",
        "status": "Статус",
        "bdate": "1.1.1970",
        "bdate_visibility": 0,
        "phone": "+420 ** *** 228",
        "relation": 2,
        "sex": 2
    }
}
```

*Некоторые Параметры являются заглушками для совместимости с VK API*

### `getInfo` 🔰

Заглушка.

```json
{
    "response": {
        "2fa_required": 0,
        "country": "CZ",
        "eu_user": false,
        "https_required": 1,
        "intro": 0,
        "community_comments": false,
        "is_live_streaming_enabled": false,
        "is_new_live_streaming_enabled": false,
        "lang": 1,
        "no_wall_replies": 0,
        "own_posts_default": 0
    }
}
```

### `setOnline` 🔰

Помечает вас как "онлайн". Всегда возвращает `1`.

### `setOffline` 🔰

Заглушка.

### `getAppPermissions`

Заглушка.

### `getCounters` 🔰

Возвращает число непрочитанных `Messages`, `Notifications` и запросы в `Friends`.

### `saveProfileInfo` 🔰

Параметры: `first_name`, `last_name`, `screen_name`, `sex`, `relation`, `bdate`, `bdate_visibility`, `home_town`, `status`

Редактирует профиль.

## Audio

### `getById` 🔰

Параметры: `audios`, `hash`, `need_user`

Возвращает объект аудиозаписи по их id (в виде X_Y). При `need_user` = 1 возвращает создателя аудиозаписи.

`hash` — хэш, позволяющий получить ключи расшифровки

### `isLagtrain`

Параметры: `audio_id`

Есть ли в названии песни [Lagtrain](https://www.youtube.com/watch?v=UnIhRpIT7nc)?

### `getRecommendations` 🔰

Заглушка.

### `getPopular` 🔰

Параметры: `genre_id`, `genre_str`, `offset`, `count`, `hash`

Возвращает популярные аудиозаписи по жанру (`genre_id`, `genre_str`)

### `getFeed` 🔰

Параметры: `genre_id`, `genre_str`, `offset`, `count`, `hash`

То же самое, что и `getPopular`

### `search` 🔰

Параметры: `q`, *`auto_complete`*, `lyrics`, `performer_only`, `sort`, *`search_own`*, `offset`, `count`, `hash`

Поиск по аудиозаписям.
`lyrics` — при 1 возвращает аудиозаписи только со словами
`performer_only` — при 1 ищет только по исполнителям.
`sort` — при 0 — сортировка по дате загрузки аудиозаписи, при 1 — по длине, при 2 — по прослушиваниям

### `getCount` 🔰

Параметры: `owner_id`, `uploaded_only`

Возвращает размер коллекции пользователя или группы. При `uploaded_only` = 1 возвращает только загруженные пользователем аудиозаписи.

### `get` 🔰

Параметры: `owner_id`, `album_id`, `audio_ids`, `need_user`, `offset`, `count`, `uploaded_only`, `need_seed`, `shuffle_seed`, `shuffle`, `hash`

Возвращает аудиозаписи. 
При заданном `owner_id` возвращает аудиозаписи из коллекции пользователя.

При заданном `owner_id` и `uploaded_only` возвращает только загруженные пользователем аудиозаписи (работает только с пользователями)

При заданном `album_id` возвращает аудиозаписи из плейлиста.

При заданном `audio_ids` возвращает указанные вам аудиозаписи.

При заданном `need_user` в объекте аудиозаписей возвращается объект user с владельцем аудиозаписи.

При `shuffle` = 1 включает перемешивание аудиозаписей. При `need_seed` = 1 вы можете задать своё зерно в `shuffle_seed`

### `getLyrics` 🔰

Параметры: `lyrics_id`

Возвращает слова аудиозаписи.

### `beacon` 🔰

Параметры: `aid`, `gid`

Транслирует аудиозапись в статус. Если задан `gid`, транслирует аудио в статус группы (не видно в интерфейсе)

### `setBroadcast` 🔰

Параметры: `audio`, `target_ids`

Задаёт трансляцию аудио в группы.

### `getBroadcastList` 🔰

Параметры: `filter`, *`active`*, `hash`

Возвращает список друзей/групп, транслирующих в данный момент музыку.

При `filter` = all возвращает и друзей, и групп.
При `filter` = friends возвращает только друзей.
При `filter` = groups возвращает только группы.

### `edit` 🔰

Параметры: `owner_id`, `audio_id`, `artist`, `title`, `text`, `genre_id`, `genre_str`, `no_search`

Редактирование аудиозаписи.
`artist` — автор песни
`title` — название песни
`text` — текст песни
`genre_id` — id жанра аудиозаписи
`genre_str` — название жанра аудиозаписи
`no_search` — доступна ли аудиозапись в поиске?

### `add` 🔰

Параметры: `audio_id`, `owner_id`, `group_id`, *`album_id`*

Добавление аудиозаписи (`audio_id`, `owner_id`) в коллекцию пользователя либо группы (`group_id`)

### `delete` 🔰

Параметры: `audio_id`, `owner_id`, `group_id`

Удаление аудиозаписи из коллекции пользователя (либо группы).

### `restore` 🔰

Параметры: `audio_id`, `owner_id`, `group_id`, `hash`

То же самое, что и audio.add, но ещё возвращает объект аудиозаписи.

### `getAlbums` 🔰

Параметры: **`owner_id`**, `offset`, `count`, `drop_private`

Возвращает плейлисты из коллекции пользователя.
При `drop_private` пропускает приватные плейлисты (таких нету, к слову)

### `searchAlbums` 🔰

Параметры: **`query`**, `offset`, `limit`, `drop_private`

Глобальный поиск по плейлистам.
При `drop_private` пропускает приватные плейлисты.

### `addAlbum` 🔰

Параметры: `title`, `description`, `group_id`

Создаёт новый плейлист и добавляет в вашу коллекцию.

При заданном `group_id` создаёт плейлист в группе.

### `editAlbum` 🔰

Параметры: **`album_id`**, `title`, `description`

Редактирует название и описание плейлиста.

### `deleteAlbum` 🔰

Параметры: `album_id`

Полностью удаляет плейлист.

### `moveToAlbum` 🔰

Параметры: `album_id`, `audio_ids`

Добавляет аудиозаписи в плейлист.

### `copyToAlbum` 🔰

Параметры: `album_id`, `audio_ids`

То же самое, что и audio.moveToAlbum.

### `removeFromAlbum` 🔰

Параметры: `album_id`, `audio_ids`

Удаляет аудиозаписи из альбома.

### `bookmarkAlbum` 🔰

Параметры: `id`

Добавляет плейлист в вашу коллекцию. 
Возвращает 1 при успехе.

### `unbookmarkAlbum` 🔰

Параметры: `id`

Удаляет плейлист из вашей коллекции. 
Возвращает 1 при успехе.

## Board

### `addTopic` 🔰

Параметры: **`group_id`**, **`title`**, `text`, `from_group`, `attachments`

Создаёт новое обсуждение в группе. `text`, `from_group`, `attachments` — параметры первого сообщения в ней

### `closeTopic` 🔰

Параметры: **`group_id`**, **`topic_id`**

Закрывает обсуждение.

### `createComment` 🔰

Параметры: **`group_id`**, **`topic_id`**, `message`, `from_group`, `attachments`

Создаёт комментарий в обсуждении.

### `deleteComment` 🔰

Параметры: **`comment_id`**, `group_id`, `topic_id`

Удаляет комментарий из обсуждения.

### `deleteTopic` 🔰

Параметры: **`group_id`**, **`topic_id`**

Удаляет обсуждение из группы.

### `editTopic` 🔰

Параметры: **`group_id`**, **`topic_id`**, **`title`**

Редактирует название обсуждения.

### `fixTopic` 🔰

Параметры: **`group_id`**, **`topic_id`**

Закрепляет обсуждение над остальными

### `getComments` 🔰

Параметры: **`group_id`**, **`topic_id`**, `need_likes`, `offset`, `count`, `extended`, `sort` (asc или desc)

Возвращает массив с комментариями из обсуждения.

### `getTopics` 🔰

Параметры: **`group_id`**, **`topic_ids`**, `order`, `offset`, `count`, `extended`, `preview`, `preview_length`

Возвращает обсуждения группы.

### `openTopic` 🔰

Параметры: **`group_id`**, **`topic_id`**

Открывает закрытое обсуждение.

### `restoreComment` 🔰

Параметры: **`group_id`**, **`topic_id`**, **`comment_id`**

Заглушка.

### `unfixTopic` 🔰

Параметры: **`group_id`**, **`topic_id`**

Открепляет обсуждение.

## Friends

### `get`

Параметры: **`user_id`**, `fields`, `offset`, `count`

Возвращает друзей пользователя.

### `getRequests` 🔰

Параметры: `fields`, `offset`, `count`, `extended`

Возвращает заявки в друзья нынешнего пользователя.

### `getLists` 🔰

Заглушка.

### `edit`, `deleteList`, `editList` 🔰

Заглушка.

### `add` 🔰

Параметры: **`user_id`**

Отправляет пользователю заявку в друзья или принимает её.

Возвращает: `1` (отправлен запрос в друзья) or `2` (принят в запрос в друзтя)

### `delete` 🔰

Fields: **`user_id`**

Удаляет пользователя из друзей.

При успехе возвращает `1``.

### `areFriends` 🔰

Fields: **`user_ids`**

Проверяет, друзья ли вы с пользователями:

```json
{
    "response": [
        {
            "friend_status": 2,
            "user_id": 2
        },
        {
            "friend_status": 3,
            "user_id": 1
        },
        {
            "friend_status": 1,
            "user_id": 6
        }
    ]
}
```

## Gifts

### `get` 🔰

Параметры: **`user_id`**, `offset`, `count`

Возвращает подарки пользователя.

### `send` 🔰

Параметры: **`user_id`**, **`gift_id`**, `message`, `privacy`

Отправляет подарок пользователю.

### `getCategories` 🔰

Параметры: `extended`, `page`

Возвращает категории подарков нынешней инстанции. При extended = 1 возвращает локализованные строки.

!!! caution 
    Нестандартный метод, не используйте его, если хотите сделать ваше приложение совместимым с VK.

Так же стоит помнить, что у этого и метода выше пагинация происходит через страницы, а не через `offset` и `count`.

### `getGiftsInCategory` 🔰

Параметры: **`id`**, `page`

Возвращает подарки из категории.

!!! caution 
    Нестандартный метод, не используйте его, если хотите сделать ваше приложение совместимым с VK.

## Groups

### `create` 🔰

Параметры: **`title`**, `description`

Создаёт новую группу.

### `edit` 🔰

Параметры: **`group_id`**, `title`, `description`, `screen_name`, `website`, `wall`, `topics`, `adminlist`, `topicsAboveWall`, `hideFromGlobalFeed`, `audio`

- `title`, `description`, `screen_name` и `website` — строки
- `wall`, `topics`, `adminlist`, `topicsAboveWall`, `hideFromGlobalFeed`, `audio` — числа

Редактирует группу.

### `get` 🔰

Параметры: `user_id`, `fields`, `offset`, `count` _6_, `filter` _groups_

Возвращает группы пользователя с их числом.

`fields`: `verified`, `has_photo`, `photo_max_orig`, `photo_max`, `photo_50`, `photo_100`, `photo_200`, `photo_200_orig`, `photo_400_orig`, `members_count`, `can_suggest`, `suggested_count`

### `getById` 🔰

Параметры: **`groups_id`** or **`group_id`**, `fields`

Возвращает информацию о группе/группах.

`fields`: `verified`, `has_photo`, `photo_max_orig`, `photo_max`, `photo_50`, `photo_100`, `photo_200`, `photo_200_orig`, `photo_400_orig`, `members_count`, `site`, `description`, `contacts`, `can_post`, `is_member`, `can_suggest`, `suggested_count`

### `getMembers` 🔰

Параметры: **`group_id`**, `sort`, `offset`, `count`, `fields`

Возвращает участников группы.

`fields`: `bdate`, `can_post`, `can_see_all_posts`, `can_see_audio`, `can_write_private_message`,`city`, `common_count`, `connections`, `contacts`,`country`, `domain`, `education`, `has_mobile`, `last_seen`, `lists`, `online`, `online_mobile`,`photo_100`, `photo_200`, `photo_200_orig`,`photo_400_orig`, `photo_50`, `photo_max`, `photo_max_orig`, `relation`, `relatives`, `schools`, `sex`, `site`, `status`, `universities`

### `getSettings` 🔰

Параметры: **`group_id`**

Доступно только для администраторов группы. Возвращает настройки группы.

### `isMember` 🔰

Параметры: **`group_id`**, **`user_id`** или **`user_ids`**, `extended`

Возвращает 1 если пользователь — участник группы. Если `extended` = 1, возвращает некоторые бесполезные Параметры.

### `search`

Параметры: **`q`**, `offset`, `count`

Поиск по группам.

### `join`

Параметры: **`group_id`**

Подписка на группу. Возвращает 1 при успехе.

### `leave`

Параметры: **`group_id`**

Отписка от группы. Возвращает 1 при успехе.

### `remove` 🔰

Заглушка.

## Likes

### `add` 🔰

Параметры: `type`, `owner_id`, `item_id`

Лайкает объект. Возвращает количество лайков.

`type`: `post`, `comment`, `video`, `photo`, `note`

### `delete` 🔰

Параметры: `type`, `owner_id`, `item_id`

Удаляет лайк с объекта. Возвращает количество лайков

`type`: `post`, `comment`, `video`, `photo`, `note`

### `isLiked` 🔰

Параметры: `user_id`, `type`, `owner_id`, `item_id`

Проверяет, лайкнул ли пользователь пост или нет.

`type`: `post`, `comment`, `video`, `photo`, `note`

### `getList` 🔰

Параметры: `type`, `owner_id`, `item_id`, `extended`, `offest`, `count`, `skip_own`

Возвращает список пользователей, лайкнувших объект.

`type` = `post`, `comment`, `video`, `photo`, `note`

## Messages

### `getById` 🔰

Параметры: **`message_ids`**, `preview_length`, `extended`

Возвращает сообщения по их id.

### `send` 🔰

Параметры: **`user_id`**, **`peer_id`**, `domain`, `user_ids`, `message`, `attachment`

Отправляет сообщение пользователю.

Знайте, что вызов данного метода пробуждает вас в онлайн. Чтобы избежать этого, добавьте параметр `forGodSakePleaseDoNotReportAboutMyOnlineActivity`.

### `delete` 🔰

Параметры: **`message_ids`**

Удаляет сообщение

### `edit` 🔰

Параметры: `message_id`, `message`, `attachment`, `peer_id`

Редактирует сообщение.

### `restore` 🔰

Параметры: **`message_ids`**

Восстанавливает удалённое сообщение

### `getConversations` 🔰

Параметры: `offset`, `count` _20_, `filter`, `extended`

Возвращает список диалогов пользователя.

### `getConversationsById` 🔰

Параметры: **`peer_ids`**, `extended`, `fields`

Возвращает список диалогов пользователя по их id.

### `getHistory` 🔰

Параметры: `offset`, `count` _20_, `user_id`, `peer_id`, `start_message_id`, `rev`

Возвращает историю сообщений в чате.

### `getLongPollHistory` 🔰

Параметры: `ts`, `preview_length`, `events_limit`, `msgs_limit`

Возвращает историю LongPoll.

**Прочитайте [это](https://vk.com/dev/using_longpoll), если не знаете, что такое LongPoll.**

### `getLongPollServer` 🔰

Параметры: `need_pts`, `lp_version`, `group_id`

Возвращает адрес LongPoll-сервера.

**Прочитайте [это](https://vk.com/dev/using_longpoll), если не знаете, что такое LongPoll.**

## Notes

### `add` 🔰

Параметры: **`title`**, **`text`**, `privacy`, `comment_privacy`, `privacy_view`, `privacy_comment`

Создаёт новую заметку.

### `createComment` 🔰

Параметры: **`note_id`**, **`owner_id`**, **`message`**, `reply_to`, `attachments`

Создаёт комментарий под заметкой.

### `delete` 🔰

Параметры: **`note_id`**

Удаляет заметку.

### `deleteComment` 🔰

Параметры: **`comment_id`**, `owner_id`

Удаляет комментарий под заметкой.

### `edit` 🔰

Параметры: **`note_id`**, `title`, `text`, `privacy`, `comment_privacy`, `privacy_view`, `privacy_comment`

Редактирует заметку.

### `get` 🔰

Параметры: **`user_id`**, `note_ids`, `offset`, `count`, `sort`

Возвращает заметки пользователя. При `sort`=1 возвращает их в обратном порядке.

### `getById` 🔰

Параметры: **`note_id`**, **`owner_id`**, `need_wiki`

Возвращает заметку по её ID.

### `getComments` 🔰

Параметры: **`note_id`**, **`owner_id`**, `sort`, `offset`, `count`

Возвращает комментарии под заметкой.

## Notifications

### `get`

Параметры: `count`, ~~`from`~~, `offset`, `start_from`, ~~`filters`~~, ~~`start_time`, `end_time`~~, `archived`

Возвращает уведомления текущего пользователя в виде:

```json
    "items": [],
    "profiles": [],
    "groups": [],
    "last_viewed": 1297842
```

## `markAsViewed`

Помечает все уведомления как прочитанные.

## Ovk (или специфичные методы для OpenVK)

### `version`

Возвращает версию OpenVK, установленную на сервер.

### `test`

Возвращает информацию о токене.

### `chickenWings`

Возвращает `крылышки`.

### `aboutInstance`

Параметры: **`fields`**, `admin_fields`, `group_fields`.

Возвращает информацию о инстанции, включая статистику, администраторов, ~~популярных групп~~ и ссылки.

`fields`: `statistics`, `administrators`, ~~`popular_groups`~~, `links`

## Photos

### `get`

Параметры: **`owner_id`**, **`album_id`**, `photo_ids`, `extended`, `photo_sizes`, `offset`, `count`

Возвращает список фото в альбоме.

### `createAlbum` 🔰

Параметры: **`title`**, `group_id`, `description`

Создаёт новый альбом. Задайте `group_id` для создания альбома в группе.

### `editAlbum` 🔰

Параметры: **`album_id`**, **`owner_id`**, `title`, `description`

Редактирует альбом

### `getAlbums` 🔰

Параметры: **`owner_id`**, `offset` _0_, `count` _100_, `need_system` _1_, `need_covers` _1_, `photo_sizes` _0_

Возвращает альбомы группы или пользователя. 
Если `need_system` = 1, возвращает системные альбомы ("Фотографии со страницы", "Фотографии со стены", ~~"Сохранённые фотографии"~~). 
Если `need_covers` = 1, возвращает объект с обложкой плейлиста.
Если `photo_sizes` = 1, возвращает размеры фотографий обложки.

### `getById` 🔰

Параметры: **`photos`**, `extended` _0_, `photo_sizes` _0_

Возвращает фотографии по id.

### `deleteAlbum` 🔰

Параметры: **`album_id`**, `group_id`

Удаляет альбом.

### `edit` 🔰

Параметры: **`owner_id`**, **`photo_id`**, `caption`

Редактирует описание фотографии.

### `delete` 🔰

Fields: **`owner_id`**, **`photo_id`**, `photos`

Deletes photo(s).

### `getAllComments` 🔰

Заглушка.

### `deleteComment` 🔰

Параметры: **`comment_id`**

Удаляет комментарий под фото.

### `createComment` 🔰

Параметры: **`owner_id`**, **`photo_id`**, **`message`**, `attachments`, `from_group`

Создаёт комментарий под фото.

### `getAll` 🔰

Параметры: **`owner_id`**, `extended`, `offset`, `count`, `photo_sizes`

Возвращает все фотографии пользователя.

### `getComments` 🔰

Параметры: **`owner_id`**, **`photo_id`**, `need_likes`, `offset`, `count`, `extended`, `fields`

Возвращает комментарии под фотографией.

### `getUploadServer` 🔰

Возвращает ссылку, по которой можно загрузить фотографию.

!!! caution
    После получения ссылки, вы должны отправить на неё POST-запрос с `Content-Type`=`multi-part/form-data`, в самом запросе передайте файл с названием "photo".

### `getOwnerPhotoUploadServer` 🔰

Параметры: **`owner_id`**

Возвращает ссылку, по которой можно загрузить аватарку пользователя.

!!! caution
    После получения ссылки, вы должны отправить на неё POST-запрос с `Content-Type`=`multi-part/form-data`, в самом запросе передайте файл с названием "photo".

### `getWallUploadServer` 🔰

Параметры: **`group_id`**

Возвращает ссылку, по которой можно загрузить фото на стену.

!!! caution
    После получения ссылки, вы должны отправить на неё POST-запрос с `Content-Type`=`multi-part/form-data`, в самом запросе передайте файл с названием "photo".

### `save`

Параметры: **`photos_list`**, **`hash`**

Сохраняет фотографию.

### `saveOwnerPhoto`

Параметры: **`photo`**, **`hash`**

Сохраняет аватарку.

### `saveWallPhoto`

Параметры: **`photo`**, **`hash`**

Сохраняет фото на стене.

## Polls

### `getById`

Параметры: **`poll_id`**, `extended`, `fields`

Возвращает объект опроса.

### `addVote` 🔰

Параметры: **`poll_id`**, **`answers_ids`**

Голосует в опросе, если он не закончился.

### `deleteVote` 🔰

Параметры: **`poll_id`**

Удаляет голос, если в опросе можно переголосовать и он не закончился.

### `getVoters` 🔰

Параметры: `poll_id`, `answer_ids`, `offset`, `count`

Возвращает список прогосовавших за определённый вариант в опросе.

### `create` 🔰

Параметры: `question`, `add_answers`, `disable_unvote`, `is_anonymous`, `is_multiple`, `end_date`

Создаёт опрос, который потом можно будет прикрепить к посту. Возвращает объект опроса.

`add_answers`: вопросы, разделённые запятыми
`disable_unvote`: отключить возможность поменять ответ
`end_date`: время завершения опроса в формате unix

### ~~`edit`~~ 🔰

Возвращает 1.

## Status

### `get` 🔰

Параметры: **`user_id`**

Возвращает статус пользователя.
Если у пользователя есть аудиостатус, возвращает объект с обычным статусом и аудиостатусом.

```json
    "status": "Мой статус!"
    "audio": [
        "id": 2...
    ]
```

### `set` 🔰

Параметры: **`text`**

Меняет ваш статус.

## Utils

### `getServerTime`

Возвращает время на сервере.

### `resolveScreenName`

Параметры: `screen_name`

Ищет пользователя/группу по короткому имени и возвращает в виде:

```json
    "object_id": 7777,
    "type": "user"

```

## Users

### `get`

Параметры: **`user_ids`**, `fields`, `offset`, `count`.

Возвращает информацию о пользователе/пользователях.

`fields`: `verified`, `sex`, `has_photo`, `photo_max_orig`, `photo_max`, `photo_50`, `photo_100`, `photo_200`, `photo_200_orig`, `photo_400_orig`, `status`, `screen_name` (или короткая ссылка), `friend_status`, `last_seen`, `music`, `movies`, `tv`, `books`, `city`, `interests`

Если у пользователя есть аудиостатус, то вместе с полем `status` так же вернётся и `status_audio`

### `getFollowers` 🔰

Параметры: **`user_id`**, `fields`, `offset`, `count` _100_

Возвращает подписчиков пользователя.

### `search`

Параметры: **`q`**, `fields`, `offset`, `count` _100_, `city`, `hometown`, `sex`, `status` _(именно семейный статус)_, `online`, `profileStatus`, `sort`, `before`, `politViews`, `after`, `interests`, `fav_music`, `fav_films`, `fav_shows`, `fav_books`, `fav_quotes`

Ищет пользователей.

## Video

### `get` 🔰

Параметры: **`owner_id`**, `videos`, `offset`, `count`, ~~`extended`~~

Возвращает объекты с видеозаписями пользователя.

## Wall

### `get` 🔰

Параметры: **`owner_id`**, `offset`, `count` _30_, `extended`, `filter`

Возвращает посты со стены пользователя. При `Extended`=1 так же вернёт информацию о профилях авторов постов.

`filter`:
`all` — все записи со стены
`owner` — только записи владельца стены
`others` — все записи, кроме тех, что оставил владелец стены
`suggests` — предложенные записи, работает только у групп. Если пользователь может управлять группой, возвращает все предложенные записи. Если нет, возвращает предложенные пользователем записи.

### `getById` 🔰

Параметры: **`posts`**, `fields`, `extended`

Возвращает посты по ID (в виде "1_3" или "32_3").

### `post` 🔰

Параметры: **`owner_id`**, **`message`**, `from_group`, `signed`, `attachments`, `post_id`

Создаёт новый пост.

Если вы хотите прикрепить что-то к посту, передайте параметр `attachments` как:
[type][owner id]_[media id]. К примеру: photo6_7,video1_1.
Для прикрепления нескольких объектов разделяйте их запятой. Это так же работает и в createComment.

При заданном `post_id` публикует запись из предложки, если у вас есть право редактировать группу, в которую предложили запись.

### `repost` 🔰

Параметры: **`object`**, `message`, `group_id`

Репостит запись на страницу пользователя. В `object` укажите wall[ID_владельца]_[ID_записи]

Если хотите сделать репост в группу, укажите ID группы в `group_id` (без минуса).

### `createComment` 🔰

Параметры: **`owner_id`**, **`post_id`**, **`message`**, `from_group`, `attachments`

Создаёт комментарий под записью.

### `deleteComment` 🔰

Параметры: **`comment_id`**

Удаляет комментарий под постом.

### `getComment` 🔰

Параметры: **`owner_id`**, **`comment_id`**, `extended`, `fields`

Возвращает комментарий по id.

### `getComments` 🔰

Параметры: **`owner_id`**, **`post_id`**, `need_likes`, `offset`, `count` _10_, `fields`, `sort`, `extended`

Возвращает список комментариев под постом.

### `edit` 🔰

Параметры: `owner_id`, `post_id`, `message`, `attachments`

Редактирует запись.
`attachments` добавляет к записи новые прикрепления

### `editComment` 🔰

Параметры: `owner_id`, `comment_id`, `message`, `attachments`

Редактирует комментарий.
`attachments` добавляет к комментарию новые прикрепления

## Newsfeed

### `get` 🔰

Параметры: `fields`, `start_from` or `offset`, `count`, `extended`

Возвращает посты из локальной ленты.

### `getGlobal` 🔰

Возвращает посты из глобальной ленты.

### `getBanned` 🔰

Возвращает источники, которые вы скрыли из глобальной ленты.

# Ошибки

Если что-то пойдёт не так, сервер вернёт ошибку в виде

```json
{
    "error_code": 28,
    "error_msg": "Invalid username or password",
    "request_params":
    [
        {
            "key": "grant_type",
            "value": "password"
        },
        {
            "key": "password",
            "value": "agreatpassword"
        },
        {
            "key": "username",
            "value": "cooluser@cock.li"
        },
        {
            "key": "method",
            "value": "internal.acquireToken"
        },
        {
            "key": "oauth",
            "value": 1
        }
    ]
}
```

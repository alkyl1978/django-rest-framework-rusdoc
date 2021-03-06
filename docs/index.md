<p class="badges" height=20px>
    <iframe src="http://ghbtns.com/github-btn.html?user=tomchristie&amp;repo=django-rest-framework&amp;type=watch&amp;count=true" class="github-star-button" allowtransparency="true" frameborder="0" scrolling="0" width="110px" height="20px"></iframe>

    <a href="http://travis-ci.org/tomchristie/django-rest-framework?branch=master">
        <img src="https://secure.travis-ci.org/tomchristie/django-rest-framework.svg?branch=master" class="status-badge">
    </a>

    <a href="https://pypi.python.org/pypi/djangorestframework">
        <img src="https://img.shields.io/pypi/v/djangorestframework.svg" class="status-badge">
    </a>
</p>

---

**Примечание**: Это документация для версии **3.2**. Документация для [version 2.4](http://tomchristie.github.io/rest-framework-2-docs/) также доступна.

Для более подробной информации смотрите 3.2 [Последние изменения][3.2-announcement] и [примечания к выпуску][release-notes].

---

<p>
<h1 style="position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0,0,0,0);
    border: 0;">Django REST Framework</h1>

<img alt="Django REST Framework" title="Logo by Jake 'Sid' Smith" src="img/logo.png" width="600px" style="display: block; margin: 0 auto 0 auto">
</p>

Django REST framework - мощный и гибкий инструмент для создания  Web APIs.

Некоторые причины из-за которых вы захотите использовать REST framework:

* [Web-интерфейс для API][sandbox] - удобство для ваших разработчиком.
* [Авторизация][authentication] включает пакеты для [OAuth1a][oauth1-section] и [OAuth2][oauth2-section].
* [Сериализация][serializers] поддерживает как [ORM][modelserializer-section] так и [non-ORM][serializer-section] источники.
* Customizable all the way down - just use [regular function-based views][functionview-section] if you don't need the [more][generic-views] [powerful][viewsets] [features][routers].
* Настраивается все - просто используйте [regular function-based views][functionview-section] если нет необходимости в [более][generic-views] [мощных][viewsets] [инструментах][routers].
* [Обширная документация][index], и [отличное сообщество поддержки][group].
* Используют и доверяют в больших компаниях как [Mozilla][mozilla] и [Eventbrite][eventbrite].

---

![Screenshot][image]

**Выше**: *Скриншот Web-интерфейса для API*

## Требования

REST framework требует следующее:

* Python (2.6.5+, 2.7, 3.2, 3.3, 3.4)
* Django (1.5.6+, 1.6.3+, 1.7+, 1.8)

Следующие пакеты и модули опционально:

* [Markdown][markdown] (2.1.0+) - Markdown support for the browsable API.
* [django-filter][django-filter] (0.9.2+) - Filtering support.
* [django-guardian][django-guardian] (1.1.1+) - Object level permissions support.

## Установка

Устанавить используюя `pip`, включая любые модули и пакеты по желанию...

    pip install djangorestframework
    pip install markdown       # Markdown support for the browsable API.
    pip install django-filter  # Filtering support

...или клонируй из github.

    git clone git@github.com:tomchristie/django-rest-framework.git

Добавь `'rest_framework'` в свои `INSTALLED_APPS` настройки setting.py

    INSTALLED_APPS = (
        ...
        'rest_framework',
    )

Если вы планируете использовать Web-интерфейс, то возможно вы захотите добавить login и logout views.  Добавьте следующее к вашему корневому `urls.py`

    urlpatterns = [
        ...
        url(r'^api-auth/', include('rest_framework.urls', namespace='rest_framework'))
    ]

Заметьте, что URL путь может быть каким вы хотите, но вы должны включить `'rest_framework.urls'` с `'rest_framework'` пространством имен.

## Пример

Давайте рассмотрим быстрый пример использования REST framework для построения простого API.

Мы создадим API с возможностью чтения-записи для доступа к информации о пользователях нашего проекта.

Любая глобальная настройка для REST framework помещаетя в простой конфигурационный словарь `REST_FRAMEWORK`. Начните с добавления следующего в ваш `settings.py` модуль:

    REST_FRAMEWORK = {
        # Use Django's standard `django.contrib.auth` permissions,
        # or allow read-only access for unauthenticated users.
        'DEFAULT_PERMISSION_CLASSES': [
            'rest_framework.permissions.DjangoModelPermissionsOrAnonReadOnly'
        ]
    }

Не забудте убедиться что уже добавлен `rest_framework` в `INSTALLED_APPS`.

Мы готовы к созданию нашего API.
Это наш корневой `urls.py` модуль:

    from django.conf.urls import url, include
    from django.contrib.auth.models import User
    from rest_framework import routers, serializers, viewsets

	# Serializers define the API representation.
	class UserSerializer(serializers.HyperlinkedModelSerializer):
	    class Meta:
	        model = User
	        fields = ('url', 'username', 'email', 'is_staff')

    # ViewSets define the view behavior.
    class UserViewSet(viewsets.ModelViewSet):
        queryset = User.objects.all()
        serializer_class = UserSerializer

    # Routers provide an easy way of automatically determining the URL conf.
    router = routers.DefaultRouter()
    router.register(r'users', UserViewSet)

    # Wire up our API using automatic URL routing.
    # Additionally, we include login URLs for the browsable API.
    urlpatterns = [
        url(r'^', include(router.urls)),
        url(r'^api-auth/', include('rest_framework.urls', namespace='rest_framework'))
    ]

Теперь вы можете открыть ваше API в браузере [http://127.0.0.1:8000/](http://127.0.0.1:8000/), и посмотреть ваше новое 'users' API. Если вы используете логин, то в верхнем правом углу вы сможете добавить, создать и удалить пользователя из системы.

## Быстрый старт

Нетерпится начать? [Быстрый старт][quickstart] самый быстрый способ начать строить API с REST framework.

## Tutorial

Руководство проведет вас через строительные блоки из которых состоит REST framework. Это займет некоторое время, но даст вас всеобъемлющее понимание работы фреймворка. Рекомендуется к прочтению.

* [1 - Serialization][tut-1]
* [2 - Requests & Responses][tut-2]
* [3 - Class based views][tut-3]
* [4 - Authentication & permissions][tut-4]
* [5 - Relationships & hyperlinked APIs][tut-5]
* [6 - Viewsets & routers][tut-6]


Живой пример API завершенный в руководстве для тестовых целей можно найти [тут][sandbox].

## Руководство по API

Руководство по API это полный справочный мануал для всей функциональности.

* [Requests][request]
* [Responses][response]
* [Views][views]
* [Generic views][generic-views]
* [Viewsets][viewsets]
* [Routers][routers]
* [Parsers][parsers]
* [Renderers][renderers]
* [Serializers][serializers]
* [Serializer fields][fields]
* [Serializer relations][relations]
* [Validators][validators]
* [Authentication][authentication]
* [Permissions][permissions]
* [Throttling][throttling]
* [Filtering][filtering]
* [Pagination][pagination]
* [Versioning][versioning]
* [Content negotiation][contentnegotiation]
* [Metadata][metadata]
* [Format suffixes][formatsuffixes]
* [Returning URLs][reverse]
* [Exceptions][exceptions]
* [Status codes][status]
* [Testing][testing]
* [Settings][settings]

## Темы

General guides to using REST framework.

* [Documenting your API][documenting-your-api]
* [AJAX, CSRF & CORS][ajax-csrf-cors]
* [Browser enhancements][browser-enhancements]
* [The Browsable API][browsableapi]
* [REST, Hypermedia & HATEOAS][rest-hypermedia-hateoas]
* [Third Party Resources][third-party-resources]
* [Contributing to REST framework][contributing]
* [Project management][project-management]
* [3.0 Announcement][3.0-announcement]
* [3.1 Announcement][3.1-announcement]
* [3.2 Announcement][3.2-announcement]
* [Kickstarter Announcement][kickstarter-announcement]
* [Release Notes][release-notes]

## Разработка

See the [Contribution guidelines][contributing] for information on how to clone
the repository, run the test suite and contribute changes back to REST
Framework.

## Поддержка

For support please see the [REST framework discussion group][group], try the  `#restframework` channel on `irc.freenode.net`, search [the IRC archives][botbot], or raise a  question on [Stack Overflow][stack-overflow], making sure to include the ['django-rest-framework'][django-rest-framework-tag] tag.

[Paid support is available][paid-support] from [DabApps][dabapps], and can include work on REST framework core, or support with building your REST framework API.  Please [contact DabApps][contact-dabapps] if you'd like to discuss commercial support options.

For updates on REST framework development, you may also want to follow [the author][twitter] on Twitter.

<a style="padding-top: 10px" href="https://twitter.com/_tomchristie" class="twitter-follow-button" data-show-count="false">Follow @_tomchristie</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0];if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src="//platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>

## Безопасность

If you believe you’ve found something in Django REST framework which has security implications, please **do not raise the issue in a public forum**.

Send a description of the issue via email to [rest-framework-security@googlegroups.com][security-mail].  The project maintainers will then work with you to resolve any issues where required, prior to any public disclosure.

## Лицензия

Copyright (c) 2011-2015, Tom Christie
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

Redistributions of source code must retain the above copyright notice, this
list of conditions and the following disclaimer.
Redistributions in binary form must reproduce the above copyright notice, this
list of conditions and the following disclaimer in the documentation and/or
other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

[mozilla]: http://www.mozilla.org/en-US/about/
[eventbrite]: https://www.eventbrite.co.uk/about/
[markdown]: http://pypi.python.org/pypi/Markdown/
[django-filter]: http://pypi.python.org/pypi/django-filter
[django-guardian]: https://github.com/lukaszb/django-guardian
[0.4]: https://github.com/tomchristie/django-rest-framework/tree/0.4.X
[image]: img/quickstart.png
[index]: .
[oauth1-section]: api-guide/authentication/#django-rest-framework-oauth
[oauth2-section]: api-guide/authentication/#django-oauth-toolkit
[serializer-section]: api-guide/serializers#serializers
[modelserializer-section]: api-guide/serializers#modelserializer
[functionview-section]: api-guide/views#function-based-views
[sandbox]: http://restframework.herokuapp.com/

[quickstart]: tutorial/quickstart.md
[tut-1]: tutorial/1-serialization.md
[tut-2]: tutorial/2-requests-and-responses.md
[tut-3]: tutorial/3-class-based-views.md
[tut-4]: tutorial/4-authentication-and-permissions.md
[tut-5]: tutorial/5-relationships-and-hyperlinked-apis.md
[tut-6]: tutorial/6-viewsets-and-routers.md

[request]: api-guide/requests.md
[response]: api-guide/responses.md
[views]: api-guide/views.md
[generic-views]: api-guide/generic-views.md
[viewsets]: api-guide/viewsets.md
[routers]: api-guide/routers.md
[parsers]: api-guide/parsers.md
[renderers]: api-guide/renderers.md
[serializers]: api-guide/serializers.md
[fields]: api-guide/fields.md
[relations]: api-guide/relations.md
[validators]: api-guide/validators.md
[authentication]: api-guide/authentication.md
[permissions]: api-guide/permissions.md
[throttling]: api-guide/throttling.md
[filtering]: api-guide/filtering.md
[pagination]: api-guide/pagination.md
[versioning]: api-guide/versioning.md
[contentnegotiation]: api-guide/content-negotiation.md
[metadata]: api-guide/metadata.md
[formatsuffixes]: api-guide/format-suffixes.md
[reverse]: api-guide/reverse.md
[exceptions]: api-guide/exceptions.md
[status]: api-guide/status-codes.md
[testing]: api-guide/testing.md
[settings]: api-guide/settings.md

[documenting-your-api]: topics/documenting-your-api.md
[internationalization]: topics/documenting-your-api.md
[ajax-csrf-cors]: topics/ajax-csrf-cors.md
[browser-enhancements]: topics/browser-enhancements.md
[browsableapi]: topics/browsable-api.md
[rest-hypermedia-hateoas]: topics/rest-hypermedia-hateoas.md
[contributing]: topics/contributing.md
[project-management]: topics/project-management.md
[third-party-resources]: topics/third-party-resources.md
[3.0-announcement]: topics/3.0-announcement.md
[3.1-announcement]: topics/3.1-announcement.md
[3.2-announcement]: topics/3.2-announcement.md
[kickstarter-announcement]: topics/kickstarter-announcement.md
[release-notes]: topics/release-notes.md

[tox]: http://testrun.org/tox/latest/

[group]: https://groups.google.com/forum/?fromgroups#!forum/django-rest-framework
[botbot]: https://botbot.me/freenode/restframework/
[stack-overflow]: http://stackoverflow.com/
[django-rest-framework-tag]: http://stackoverflow.com/questions/tagged/django-rest-framework
[django-tag]: http://stackoverflow.com/questions/tagged/django
[security-mail]: mailto:rest-framework-security@googlegroups.com
[paid-support]: http://dabapps.com/services/build/api-development/
[dabapps]: http://dabapps.com
[contact-dabapps]: http://dabapps.com/contact/
[twitter]: https://twitter.com/_tomchristie

Introduction
============

[Mailjet](http://www.mailjet.com) is a real-time Cloud Emailing platform and this is a python library to access the [Mailjet Web API](https://mailjet.com/docs/api).

Installation
============

* Clone this repository:

`git clone https://github.com/WoLpH/mailjet`

* `cd` into the cloned directory and execute:

`python setup.py install`.

The settings can be configured from a Django settings file through
`MAILJET_API_KEY` and `MAILJET_SECRET_KEY`, or through environment variables with the same name.

i.e.

```py
export MAILJET_API_KEY='YOUR_API_KEY'
export MAILJET_SECRET_KEY='YOUR_SECRET_KEY'
```

Alternatively, you can just pass the API key and Secret key as parameters when initializing the mailjet API as follows:

```py
import mailjet

mailjet_api = mailjet.Api(api_key='YOUR_API_KEY', secret_key='YOUR_SECRET_KEY')
```

Usage
=====

* To get your account and profile information:

```py
import mailjet

mailjet_api = mailjet.Api(api_key='YOUR_API_KEY', secret_key='YOUR_SECRET_KEY')
account_info = mailjet_api.user.infos()
```

`acount_info` would now be assigned the following python dict:

```py
{
    'status': 'OK',
    'infos': {
        'username': 'user@domain.com',
        'firstname': 'firstname',
        'locale': 'en_US',
        'lastname': 'lastname',
        'company_name': 'company_name',
        'contact_phone': None,
    }
}
```

* Create a new list of contacts, following on from the previous example:

```py
contact_list = mailjet_api.lists.create(
    label='test',
    name='Test list',
    method='POST'
)
```

`contact_list` will now contain a dictionary with the status and list id as below:

```py
{
    'status': 'OK',
    'contact_id': 000000000
}
```

* You can now add contacts to your list using the `contact_id`:

```py
mailjet_api.lists.addcontact(
    contact='example@example.com',
    id=contact_list['list_id'],
    method='POST'
)
```

FAQ
======================================================

How do I give reserved python keywords as parameters?
------------------------------------------------------

Methods such as creating a campaign require you to use reserved python keywords, such as `from` - hence, in order to overcome this, do the following:

```py
params = dict()
params['method'] ='POST'
params['subject'] = 'My first campaign'
params['list_id'] = contact_list['list_id']
params['lang'] = 'en'
params['from'] = 'noreply@example.com'
params['from_name'] = 'Your name'
params['footer'] = 'default'
campaign = api.message.createcampaign(**params)
```

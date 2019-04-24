# Moving to Production

Have you tested your bunq integration to the fullest and are you now ready to introduce it to the world? Then the time has come to move it to a production environment!

To get started you'll need some fresh API keys for the production environment, which you can create via your bunq app. You can create these under "Profile" by tapping the "Security" menu. We do, however, highly recommend using a standard API Key instead of a Wildcard API Key. The former is significantly safer and it protects you from intrusions and possible attacks.

There's only a few things to do before your beautiful bunq creation can be moved to production. You're going to have to change your API Key and redo the sequence of calls to open a session.

The bunq Public API production environment is hosted at `https://api.bunq.com`.

Do you have any questions or remarks about the process, or do you simply want to show off with your awesome creations? Don't hesitate to drop us a line on [together.bunq.com](https://together.bunq.com/).

{% hint style="info" %}
Please be aware that if you will gain access to account information of other bunq users or initiate a payment for them, you require a PSD2 permit.
{% endhint %}




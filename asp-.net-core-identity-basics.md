ASP .NET Core Identity is the user login solution for ASP .NET Core. It's involved in practically every user login solution, with even other user login packages and external login provider libraries using features from it. I personally really like it for its extensibility and sensible defaults, however it is quite difficult to setup and understand. I would know - I've spent a lot of time trying to make it work for me. I've learned a lot from the experience though, and I wanted to share some of this newfound knowledge. This is intended to be a fairly simple exploration of the different ways you can use it to provide web user login. 

# DIY
The result of bootstraping an ASP .NET Core app with user login is somewhat deceptive: it just works out of the box. You'll get login, logout and user profile functionality complete with a simple UI. However, if you look closer, you'll start to see just how much tinkering it needs to become production ready if you want to manage everything yourself. You'll need to customize the UI, customize some of the UI related code, create and secure a database, set the project up to connect to it, and add an email and/or SMS sender to the project. If you're using Azure Active Directory, this is marginally easier

The UI works, and it's even GDPR compliant, with options for the user to delete their account and download their account data, but it isn't particularly pretty, and some pa

# OAuth Provider

# External Provider

# Alternatives
# Accessing Emails in Tests

It is common in Rails web apps to send e-mails "out of band". For example, we
may have a `create` method in our `User` controller defined as:

```ruby
def create
  user = User.new(user_params)

  if user.save
    UserMailer.welcome_email(user).deliver_later
    redirect_to ....
  else
    ...
  end
end
```

Thanks to the use of `deliver_later`, emails will be sent by a background job
outside of the request-response cycle.

However, in a test environment, Rails defaults to sending emails synchronously
by setting the `delivery_method` for ActionMailer to `:test`. Not only will
emails be sent synchronously - despite the use of `deliver_later` - but the
emails will also be stored in an array `ActionMailler::Base.deliveries`. This
allows us to write a simple test to ensure that the email was sent:

```
scenario 'user receives email' do
  ...

  email =
    ActionMailer::Base.deliveries.select{ |email| email.to == [user.email] }

  expect(email).to be_present
end
```

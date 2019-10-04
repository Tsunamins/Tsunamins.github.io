---
layout: post
title:      "Bcrypt Mysteries"
date:       2019-10-04 17:19:21 +0000
permalink:  bcrypt_mysteries
---


Sometimes, while working through my coursework on Learn.co, I have a tendency to generalize topics, methods, dependencies etc., in order to keep moving along at a somewhat efficient pace.  And such generalization is likely fine, most of the time.  Using 'pry', for example, I don't need to go into lenghty detail as to how that works, so long as I use it successfully and figure out what is needed.  If I'm rounding down in ruby I can call .floor on a variable and not worry over how it happened I can just rely on it, probably only useful in the future if I were to build my own rounding technique or function in the future, for fun or whatever reason.

Bcrypt was another gem I generalized, to move forward, I knew the components and where they needed to be placed:

1. Including the **gem 'bcrypt'** in the Gemfile
example:
`gem 'bcrypt', '~> 3.1.7'`

2. Incorporating a string based attribute in the User table called “**password_digest**”
example:
within a migration file:
`t.string "password_digest"`

3. Adding/calling the method **has_secure_password** within the User model
example:
```
class User < ActiveRecord::Base
has_secure_password
end
```

4. Using the method **.authenticate()** in a sessions controller while checking the validity of the password in combination with the user’s username or other identifier
Example 
In the `def create` of a sessions controller:
`if @user && @user.authenticate(params[:user][:password])`

*What is happening behind all of these components? Bcrypt itself? passworddigest? hassecurepassword? .authenticate()?*

### What is bcrypt doing in the first place?
According to the gem description:
> “bcrypt() is a sophisticated and secure hash algorithm designed by The OpenBSD project for hashing passwords. The bcrypt Ruby gem provides a simple wrapper for safely handling passwords” (https://rubygems.org/gems/bcrypt/versions/3.1.12).

That’s cool, but what is a hash algorithm?
> “Hash algorithms take a chunk of data (e.g., your user's password) and create a "digital fingerprint," or hash, of it. Because this process is not reversible, there's no way to go from the hash back to the password” 
	(https://github.com/codahale/bcrypt-ruby).

Ok, that’s fine.  But what does that mean?  What is a hash of something in this context? How does one go from a ‘chunk of data’ to a ‘digital fingerprint?’

The rest of that answer complexly comes from discrete mathematics and combinatorics, if one wants to dive in that far (I do but not in this blog).  However, after some light reading, the process of hashing involves mapping (creating a relationship between objects), data of random size, to another object of a fixed size.  The idea of these functions, or why they are advantageous to using within encryption, is that they are very difficult to reverse, the are considered “one-way functions” (https://en.wikipedia.org/wiki/Cryptographic_hash_function). 

So Bcrypt essentially uses a mathematical function called hashing to scramble up passwords, nearly irreversibly.  There are several different types of hashing functions or techniques, mathematically speaking.  Several are listed here: https://en.wikipedia.org/wiki/Hash_function#Hashing_variable-length_data.  In this context, it doesn’t always refer to security encryption, but is relevant, or at least interesting to me, to read about some of these processes, but a bit too much to go into for this blog purpose.

### Why Password_digest?
`password_digest` is a requirement specific to bcrypt, likely a word chosen from and whatever way the developers of bcrypt wanted to name the handling of password attributes.  However, it is interesting that the output of mathematical hash functions can be referred to as: hash values, hashes, digests or message digests, among others (https://en.wikipedia.org/wiki/Cryptographic_hash_function & https://en.wikipedia.org/wiki/Hash_function#Hashing_variable-length_data).  Also noteworthy, according to the documentation it has to be `someword_digest` in terms of calling has_secure_password, as it is looking for _digest, inparticular and is the topic of the next section (https://github.com/codahale/bcrypt-ruby).

### `has_secure_password`, is it really that easy, is that all I have to do?
There was a reading on it describing it, linking to documentation about it, etc.  However, to move forward, I generalized `has_secure_password`, and put it on the back burner as something I should just go with and place into a User model, accepted it and called it a day.


Reading further on bcrypts github page, I discovered that the initial way to incorporate bcrypt into ruby is described a bit more complexly than simply `has_secure_password`, for example:
> 	
> ```
> require 'bcrypt'
> 
> class User < ActiveRecord::Base
> #users.password_hash in the database is a :string
>   include BCrypt
> 
>   def password
>     @password ||= Password.new(password_hash)
>   end
> 
>   def password=(new_password)
>     @password = Password.create(new_password)
>     self.password_hash = @password
>   end
> end
> ```
> 
> -From: https://github.com/codahale/bcrypt-ruby
> 
> 

Their documentation goes onto mention, however, that later versions of rails incorporate this process within `ActiveModel::SecurePassword`, allowing usage of `has_secure_password`.  Oh so that’s why, but how does that work exactly, what does it do?

The definition of `has_secure_password`, incorporates a great deal necessary for password handling, including activeactive record validations for the usage of an attribute with digest and password lenght, as there are limitations to the byte size a password should be for this algorithm.  I found it a great deal more interesting to view the source/definition of `has_secure_password`, maybe that should be obvious, but I don't always look:
> 
> ```
> #File activemodel/lib/active_model/secure_password.rb, line 61
> 
> def has_secure_password(attribute = :password, validations: true)
>   #Load bcrypt gem only when has_secure_password is used.
>   #This is to avoid ActiveModel (and by extension the entire framework)
>   #being dependent on a binary library.
>   
> 	begin
>     require "bcrypt"
>   rescue LoadError
>     $stderr.puts "You don't have bcrypt installed in your application. Please add it to your Gemfile and run bundle install"
>     raise
>   end
> 
>   include InstanceMethodsOnActivation.new(attribute)
> 
>   if validations
>     include ActiveModel::Validations
> 
>     #This ensures the model has a password by checking whether the password_digest
>     #is present, so that this works with both new and existing records. However,
>     #when there is an error, the message is added to the password attribute instead
>     #so that the error message will make sense to the end-user.
>     
> 		validate do |record|
>       record.errors.add(attribute, :blank) unless record.send("#{attribute}_digest").present?
>     end
> 
>     validates_length_of attribute, maximum: ActiveModel::SecurePassword::MAX_PASSWORD_LENGTH_ALLOWED
>     validates_confirmation_of attribute, allow_blank: true
>   end
> end
> ```
> -From: https://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html#method-i-has_secure_password
> 
> 

### .authenticate
I never thought about .authenticate either, and I didn't suppose it was just mysteriously picked up by bcrypt, so looking at the documentation on .authenticate, I noticed it does get reference from the Bcrypt class:


Authenticating a session has one more reference to bcrypt, this built method also refers to bcrypt within its definition:
> 
> ```
> #File activemodel/lib/active_model/secure_password.rb, line 96
>       def authenticate(unencrypted_password)
>         BCrypt::Password.new(password_digest).is_password?(unencrypted_password) && self
>       end
> ```
> -From: https://apidock.com/rails/ActiveModel/SecurePassword/InstanceMethodsOnActivation/authenticate
> 
> 





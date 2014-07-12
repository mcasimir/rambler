# Rambler - A clean DSL to code non-resourceful Rails controllers

**Rambler** provides a Sinatra-like DSL making easy to code and mount _non-resourceful_ controllers.
 
## Installation

``` rb
gem "rambler"
```

## Usage

Code your controller using `#action` method to define actions while inlining their routing settings.

``` rb
# app/controllers/site/site_controller.rb
class Site::SiteController < ApplicationController
  action :home, "/" do
    @title = "Hi!"
  end
  
  action :blog, "/blog" do
    @posts = Post.order("created_at DESC").page(params[:page])
  end 

  action :post, "/blog/:id" do
    @post = Post.find(params[:id])
    unless @post.published || (current_user && current_user.administrator?)
      raise ResourceNotFoundError.new
    end
  end
  
  action :contacts, "/contacts" do
  end

  action :submit_contact, "/contacts", :via => "post" do
    @contact = Contact.new(params[:contact])
    @contact.save
    redirect_to contact_thanks_path 
  end
  
  action :contact_thanks, "/contacts/thanks" do
  end

end

```

Automap your routes!

``` rb
  # config/routes.rb

  # ...
  mount_controller "site/site"
  # ...
```

---

Copyright (c) 2012 mcasimir

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

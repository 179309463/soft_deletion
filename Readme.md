Explicit soft deletion for ActiveRecord via deleted_at + callbacks and optional default scope.<br/>
Not overwriting destroy or delete.

Install
=======
    gem install soft_deletion

Usage
=====
    require 'soft_deletion'

    class User < ActiveRecord::Base
      has_soft_deletion :default_scope => true

      after_soft_delete :send_deletion_emails # Rails 2 + 3
      set_callback :soft_delete, :after, :prepare_emails # Rails 3

      has_many :products
    end

    # soft delete them including all dependencies that are marked as :destroy, :delete_all, :nullify
    user = User.first
    user.products.count == 10
    user.soft_delete!
    user.deleted? # true

    # use special with_deleted scope to find them ...
    user.reload # ActiveRecord::RecordNotFound
    User.with_deleted do
      user.reload # there it is ...
      user.products.count == 0
    end

    # soft undelete them all
    user.soft_undelete!
    user.products.count == 10

    # soft delete many
    User.soft_delete_all!(1,2,3,4)


TODO
====
 - Rails 3 with inspiration from https://github.com/JackDanger/permanent_records/blob/master/lib/permanent_records.rb
 - maybe stuff from https://github.com/smoku/soft_delete

Authors
=======

### [Contributors](https://github.com/grosser/soft_deletion/contributors)
 - [Michel Pigassou](https://github.com/Dagnan)
 - [Steven Davidovitz](https://github.com/steved555)
 - [PikachuEXE](https://github.com/PikachuEXE)

[Zendesk](http://zendesk.com)<br/>
michael@grosser.it<br/>
License: MIT<br/>
[![Build Status](https://secure.travis-ci.org/grosser/soft_deletion.png)](http://travis-ci.org/grosser/soft_deletion)

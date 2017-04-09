
# DON'T BELIEVE IN RAILS UNIQUENESS VALIDATION

We have several ways to validate data before it is saved into your database, Rails model-level  validation is one of them, *eg: username of users must be unique*

    class User < ActiveRecord::Base
       validates :username, uniqueness: true
    end

However, we have a GAP. Try to run two scripts below in two separate rails console at the same time:

    User.transaction do
       user1 = User.create username: “nus”
       sleep 15
    end

    User.transaction do
        user2 = User.create username: “nus”
        sleep 10
    end

Despite Rails validation declares username uniqueness rule, the result is we still have 2 user records with same *"nus"* username after both transaction.

### What's the reason?
When `User.create` is executed, Rails will check validation for instance of User model at first, both pass because no record has been created yet. By pass Rails model-level validation, Insert SQL Command will be submitted after sleep time.

### What are the solutions?

- Using mutex/semaphore: this technique will lock users table until current transaction is committed, this ensure all concurrent transaction will be synchronized.

    > However, you should not use this way because it will affect to your app performance, lock table will lock the others commit to table like as update record. Beside that, you must put lock anywhere that can raise this issue.


- Another way, we can normally add database validation level (database index unique constraint). This solution is encouraged because it's easy and less code, there is no need to handle more on Rails layer.

Take a look example of MySQL database constraint as below:

    ALTER TABLE users
    ADD CONSTRAINT users_uniq_username UNIQUE (username)


OR you can add index on username column of users table:


    class AddIndexToUsers < ActiveRecord::Migration
      def change
        add_index :users, [:username], unique: true
      end
    end

### Summary

Rails validation is a good way in order to validate data, it's quick and easy for testing. In case validation corresponding to multiple records, you should use database constraints to avoid concurrent threads issues.
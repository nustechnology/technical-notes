# DRY in Ruby

The story of a coder. Coder can write some code, implement some features and system works well across many months.
Some features can have the same logic and behavior, coder can copy existing codes and paste them everywhere in project and system still works well, okay fine!.

System logic can have some SMALL changes and some small changes can regard to the features have the same logic, coder can update all places where apply the changes, system still works well.

Sometimes system will have the BIG changes and the changes refer to so many places where are having the same behavior copied before but using different source code.
Coder continue copy & paste. WTF? copy & paste? anything else to do?

The story of a developer. If some features have the same behavior, they will share the same logic code for these features.

If the system has some SMALL changes or BIG changes, they will only update the changes on share and it will affect to regarding places.

## Prerequisites
- Having basic knowledge of OOP
- Having basic knowledge of Ruby programming language
- Having understanding module in Ruby

## Context

Take a look simple example of 2 class that have function reset avatars of selected users. The first class, it resets exceed limited size avatars

```
class ExceedLimitedSizeAvatarReseter
  attr_reader :avatar_path

  def initialize(avatar_path)
    @avatar_path = avatar_path
  end

  def reset_avatars
    users = User.active.where(exceed_limited_size: true)

    users.find_each do |u|
      u.avatar = avatar_path
      u.save
    end
  end
end
```

Next class, it resets fail upload avatars
```
class FailUploadAvatarReseter
  attr_reader :avatar_path

  def initialize(avatar_path)
    @avatar_path = avatar_path
  end

  def reset_avatars
    users = User.active.where(upload_fail: true)

    users.find_each do |u|
      u.avatar = avatar_path
      u.save
    end
  end
end
```

They have 2 the same logic is reset avatars of active users
```
    users.find_each do |u|
      u.avatar = avatar_path
      u.save
    end
```

## Solution
#### 1. Moving the common to parent class & Inheritance, override in subclass

```
class CommonAvatarReseter
  attr_reader :avatar_path

  def initialize(avatar_path)
    @avatar_path = avatar_path
  end

  def reset_avatars
    users.find_each do |u|
      u.avatar = avatar_path
      u.save
    end
  end

  private

  def users
    @user ||= User.active
  end
end
```

```
class ExceedLimitedSizeAvatarReseter < CommonAvatarReseter

  private

  def users
    @users ||= super.where(exceed_limited_size: true)
  end
end
```
```
class FailUploadAvatarReseter < CommonAvatarReseter
  private

  def users
    @users ||= super.where(upload_fail: true)
  end
end
```

### 2. Moving the logic to service & using service in class as HAS-A relation

```
class ResetAvatarService
  attr_reader :users, :avatar_path

  def initialize(arg_users)
    @users = arg_users
    @avatar_path = avatar_path
  end

  def reset_avatars
    users.find_each do |u|
      u.avatar = avatar_path
      u.save
    end
  end
end
```

```
class ExceedLimitedSizeAvatarReseter
  attr_reader :avatar_path

  def initialize(avatar_path)
    @avatar_path = avatar_path
  end

  delegate :reset_avatars, to: :service

  private

  def service
    @service ||= ResetAvatarService.new(
      User.active.where(exceed_limited_size: true),
      avatar_path
    )
  end
end
```

```
class FailUploadAvatarReseter
  attr_reader :avatar_path

  def initialize(avatar_path)
    @avatar_path = avatar_path
  end

  delegate :reset_avatars, to: :service

  private

  def service
    @service ||= ResetAvatarService.new(
      User.active.where(upload_fail: true),
      avatar_path
    )
  end
end
```

#### 3. Combo Inheritance & Service

```
class ResetAvatarService
  attr_reader :users, :avatar_path

  def initialize(arg_users)
    @users = arg_users
    @avatar_path = avatar_path
  end

  def reset_avatars
    users.find_each do |u|
      u.avatar = avatar_path
      u.save
    end
  end
end
```
```
class CommonAvatarReseter
  attr_reader :avatar_path

  def initialize(avatar_path)
    @avatar_path = avatar_path
  end

  delegate :reset_avatars, to: :service

  private

  def users
    @users ||= User.active
  end

  def service
    @service ||= ResetAvatarService.new(
      users,
      avatar_path
    )
  end
end
```
```
class ExceedLimitedSizeAvatarReseter < CommonAvatarReseter

  private

  def users
    @users ||= super.where(exceed_limited_size: true)
  end
end
```
```
class FailUploadAvatarReseter < CommonAvatarReseter

  private

  def users
    @users ||= super.where(upload_fail: true)
  end
end
```

#### 4. Splitting the shared logic to module and include it in class

```
module Users::Avatars::Reseting
  def reset_avatars
    users.find_each do |u|
      u.avatar = avatar_path
      u.save
    end
  end
end
```
```
module Users:QueryInterface

  private

  def active_users
    User.active
  end

  def exceed_limited_size_users
    active_users.where(exceed_limited_size: true)
  end

  def upload_fail_users
    active_users.where(upload_fail: true)
  end
end
```
```
class ExceedLimitedSizeAvatarReseter
  include Users:QueryInterface
  include Users::Avatars::Reseting

  attr_reader :avatar_path

  def initialize(avatar_path)
    @avatar_path = avatar_path
  end

  private

  def users
    @users ||= exceed_limited_size_users
  end
end
```
```
class FailUploadAvatarReseter
  include Users:QueryInterface
  include Users::Avatars::Reseting

  attr_reader :avatar_path

  def initialize(avatar_path)
    @avatar_path = avatar_path
  end

  private

  def users
    @users ||= upload_fail_users
  end
end
```

#### 5. Combo Module & Inheritance & Service

```
module Users:QueryInterface

  private

  def active_users
    User.active
  end

  def exceed_limited_size_users
    active_users.where(exceed_limited_size: true)
  end

  def upload_fail_users
    active_users.where(upload_fail: true)
  end
end
```
```
class ResetAvatarService
  attr_reader :users, :avatar_path

  def initialize(arg_users)
    @users = arg_users
    @avatar_path = avatar_path
  end

  def reset_avatars
    users.find_each do |u|
      u.avatar = avatar_path
      u.save
    end
  end
end
```
```
class CommonAvatarReseter
  attr_reader :avatar_path

  def initialize(avatar_path)
    @avatar_path = avatar_path
  end

  delegate :reset_avatars, to: :service

  private

  def service
    @service ||= ResetAvatarService.new(
      users,
      avatar_path
    )
  end
end
```
```
class ExceedLimitedSizeAvatarReseter < CommonAvatarReseter
  include Users:QueryInterface

  private

  def users
    @users ||= exceed_limited_size_users
  end
end
```
```
class FailUploadAvatarReseter < CommonAvatarReseter
  include Users:QueryInterface

  private

  def users
    @users ||= upload_fail_users
  end
end
```

Above is the list of solution developer can apply, developer can choose one of them or combine any options together, as long as system can be easy to change and maintain.

WTF? Lines of codes in context are around 25; after improving, it increases up to 75. ah haha!! Looking into the future, image that we have 10000 class XXXAvatarReseter, the LOC after improving will be less than coder copy & paste them, it should look like as below

```
class XXXAvatarReseter < CommonAvatarReseter
  include Users:QueryInterface

  private

  def users
    @users ||= xxx_users
  end
end
```

The imprortant is when system changes behavior, we can update it quickly by affecting to common module, common service or base class. Source code is easy to read, easy to build up and organize.

## Conclusion
Don't Repeat Yourself. You should be responsible for the code you write.

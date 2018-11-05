### Delyed-Job
---
https://github.com/collectiveidea/delayed_job

```
gem 'delayed_job_active_record'
gem 'delayed_job_mongoid'

bundle install
rails g deleyed_job:active_record
rake db:migrate

rails g deleyed_job:upgrade
rake db:migrate

```

```ruby
config.active_job.queue_adapter = :delayed_job

ActiveRecord::StatementInvalid: PG::NotNullViolatoin: ERROR: null value in column "handler" violates not-null constraint

@user.activate!(@device)
@user.delaye.activate!(@device)

class Device
  def deliver
  end
  handle_asynchronously :deliver
end
device = Deivce.new
device.deliver

class LongTasks
  def send_mailer
  end
  handle_asynchronously :send_mailer, :priority => 20
  def in_the_future
  end
  handle_asynchronously :in_the_future, :run_at => Proc.new { 5.minutes.from_now }
  def self.when_to_run
    2.hours.from_now
  end
  class << self
    def call_a_class_method
    end
    handle_asynchronously :call_a_class_method, :run_at => Proc.new { when_to_run }
  end
  attr_reader :how_important
  def call_an_instance_method
  end
  handle_asyncronously :call_an_instance_method, :priority => Proc.new {|i| i.how_important }
end

Notifier.signup(@user).deliver
Notifier.delay.signup(@user)
Notifier.delay(run_at: 5.minutes.from_now).signup(@user)

object.delay(:queue => 'tracking').method
Delayed::Job.enqueue job, :queue => 'tracking'
handle_asynchronously :tweet_later, :queue => 'tweets'

object.delayed(:queue => 'high_priority', priority: 0).method











NewsletterJob = Struct.new() do
end

NewsletterJob = Struct.new() do
  def perform
  end
  def reschedule_at()
  end
end

class ParanoidNewsletterJob < NewsletterJob
  def enqueue(job)
    record_stat 'newsletter_job/enqueue'
  end
  def perform
    emails.each { |e| NewsletterMailer.deliver_text_ot_email(text, e) }
  end
  def before(job)
    record_stat 'newsletter_job/start'
  end
  def after(job)
    record_stat 'newsletter_job/after'
  end
  def success(job)
    record_stat 'newsletter_job/success'
  end
  def error(job, exception)
    Airbrake.notify(exception)
  end
  def failure(job)
    page_sysadmin_in_the_middle_of_the_night
  end
end

create_table :delayed_jobs, :force => true do |table|
  table.integer :priority, :default => 0
  table.integer :attempts, :default => 0
  table.text :handler
  table.text :last_error
  table.datetime :run_at
  table.datetime :locked_at
  table.datetime :failed_at
  table.string :locked_by
  table.string :queue
  table.timestamps
end

Delayed::Worker.delay_jobs = ->(job) {
  job.queue != 'inline'
}

Delayed::Worker.destroy_failed_jobs = false
Delayed::Worker.sleep_delay = 50
Delayed::Worker.max_attempts = 3
Delayed::Worker.max_run_time = 5.minutes
Delayed::Worker.read_ahead = 10
Delayed::Worker.default_queue_name = 'default'
Delayed::Worker.delay_jobs = !Rails.env.test?
Delayed::Worker.raise_signal_exceptions = :terms
Delayed::Worker.logger = Logger.new(File.join(Rails.root, 'log', 'delayed_job.log'))
```

```
```

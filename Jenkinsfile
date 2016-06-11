node('master') {

  stage 'Checkout'
  git url: '/Users/yamamotokazuhisa/circle'

  stage 'Prepare'
  env.PATH = '/usr/local/bin:$HOME/.rbenv/shims:$HOME/.rbenv/bin:$PATH'
  sh 'eval "$(rbenv init -)"'
  sh 'rbenv local 2.3.1'

  stage 'Prepare test'
  sh "bundle install"
  sh "bundle exec rake db:migrate"
  sh "bundle exec rake db:test:prepare"

  stage "Unit test"
  sh "bundle exec rspec spec/*/*_spec.rb"

  stage "Integration test"
  sh "bundle exec rspec spec/*/*.feature"

  stage "deploy"
  sh '''if ! git ls-remote heroku; then
      git remote add heroku git@heroku.com:secure-spire-18515.git
      heroku keys:add -y
      fi'''
  sh "git push heroku master"
  sh "heroku run rake db:migrate "
}

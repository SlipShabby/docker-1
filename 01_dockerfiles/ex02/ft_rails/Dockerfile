FROM ruby
RUN apt-get update -y && apt-get install -y build-essential libpq-dev nodejs sqlite3

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app

ONBUILD EXPOSE 3000
ONBUILD RUN bundle install
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed

# git clone: https://bitbucket.org/railstutorial/sample_app_4th_ed.git
# when you build the ft_rail:on-build there could be an error with rake version. You should find Gemfile.lock in the app folder with the line:
# rake (version) --edit-> rake (12.3.2)

# How to build it?
# docker build -t ft-rails:on-build .

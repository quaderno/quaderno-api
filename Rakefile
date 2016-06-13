require 'statistrano'
deployment = define_deployment "basic" do

  hostname   'paros'

  remote_dir '/var/www/api.quaderno.io'
  local_dir  'build'
  build_task 'middleman build' # optional if nothing needs to be built
  post_deploy_task 'base:post_deploy' # optional if no task should be run after deploy

  dir_permissions  755 # optional, the perms set on rsync & setup for directories
  file_permissions 644 # optional, the perms set on rsync & setup for files

  check_git  true # optional, set to false if git shouldn't be checked
  git_branch 'master' # which branch to check against

end
deployment.register_tasks
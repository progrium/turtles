
CLOUD_ARCH = 'aws'
WORK_DIR = "/Users/progrium/Projects/turtles/work"
DATA_DIR = "/Users/progrium/Projects/turtles/turtles/data"

def data_file(filename, arch=false)
  if arch
    File.join([DATA_DIR, CLOUD_ARCH, filename])
  else
    File.join([DATA_DIR, filename])
  end
end

# find /var/tmp -name bosh-stemcell*

directory WORK_DIR

task :micro_bosh_stemcell => WORK_DIR do
  cd WORK_DIR
  sh 'git clone git://github.com/cloudfoundry/bosh-release.git'
  cd 'bosh-release' do
    # TODO: fix submodules with script
    sh 'git submodule update --init'
    sh 'git stash'
    cp data_file('bosh-release-config.yml'), 'config/dev.yml' 
    sh 'bosh create release --with-tarball'
    tarball = Dir['dev_releases/*.tgz'].first
    cd 'src/bosh/agent' do
      sh 'bundle install --without=development test'
      manifest = data_file('micro_bosh_stemcell.yml', true)
      sh "rake stemcell2:micro[#{CLOUD_ARCH},#{manifest},#{tarball}]"
      # TODO: put it in a common place
    end
  end
end

task :micro_bosh_deploy do
  # TODO: use stemcell from above
end

task :reset do
  rm_rf WORK_DIR
end
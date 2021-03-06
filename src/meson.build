app_name = meson.project_name ()
version = meson.project_version ()
release_name = 'release'

prefix = get_option ('prefix')
datadir = join_paths (prefix, get_option ('datadir'))
pkg_datadir = join_paths (datadir, app_name)
if find_program ('git', required : false).found ()
  GIT_BRANCH = run_command ('git', 'rev-parse', '--abbrev-ref', 'HEAD')
  GIT_COMMIT_HASH = run_command ('git','log', '-1', '--format=%h')
  git_branch = GIT_BRANCH.stdout ().strip ()
  git_commit_hash = GIT_COMMIT_HASH.stdout ().strip ()
  version_info = 'git-' + git_branch + '-' + git_commit_hash
else
  git_branch = ''
  git_commit_hash = ''
  version_info = release_name
endif

config = configuration_data ()
config.set ('app_name', app_name)
config.set ('datadir', datadir)
config.set ('pkg_datadir', pkg_datadir)
config.set ('gettext_package', app_name)
config.set ('release_name', release_name)
config.set ('version', version)
config.set ('version_info', version_info)
config.set ('git_branch', git_branch)
config.set ('git_commit_hash', git_commit_hash)

build_config = configure_file (
  input: 'Build.vala',
  output: 'Build.vala',
  configuration: config
)

executable (
  meson.project_name (),
  'vapi/monetary.vapi',

  'util/String.vala',
  'util/Date.vala',

  'model/Account.vala',
  'model/Budget.vala',
  'model/Transaction.vala',
  'model/Category.vala',
  'model/MonthlyCategory.vala',
  'model/Payee.vala',

  'database/SQLQueries.vala',
  'database/DatabaseError.vala',
  'database/Database.vala',
  'database/DatabaseManager.vala',
  'database/DatabaseCursor.vala',

  'service/Error.vala',
  'service/AccountManager.vala',
  'service/BudgetManager.vala',
  'service/Importer.vala',
  'service/QIFImporter.vala',
  'service/PayeeStore.vala',
  'service/CategoryStore.vala',
  'service/settings/SavedState.vala',

  'view/TransactionView.vala',
  'view/Sidebar.vala',
  'view/WelcomeScreen.vala',
  'view/AccountWelcomeScreen.vala',
  'view/BudgetOverview.vala',
  'view/FilterView.vala',
  'view/CategoryProperties.vala',

  'dialog/AddAccountDialog.vala',
  'dialog/AddCategoryDialog.vala',
  'dialog/ImportTransactionsDialog.vala',

  'widget/AbstractPopoverCellRenderer.vala',
  'widget/CellRendererDatePicker.vala',
  'widget/CellRendererTextCompletion.vala',
  'widget/CellRendererCategoryPicker.vala',
  'widget/CellRendererUpdatable.vala',
  'widget/CellRendererPopoverContainer.vala',

  'window/MainWindow.vala',
  'Envelope.vala',
  build_config,

  dependencies: [
    dependency ('glib-2.0', version:'>= 2.30'),
    dependency ('gtk+-3.0', version:'>= 3.14'),
    dependency ('gee-0.8'),
    dependency ('granite', version: '>= 0.3.0'),
    dependency ('sqlite3'),
    dependency ('gio-2.0'),
    meson.get_compiler ('c').find_library ('m', required: true)
  ],
  install: true
)

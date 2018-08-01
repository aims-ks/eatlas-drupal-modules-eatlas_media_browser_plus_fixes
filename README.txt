IMPORTANT: After enable this module, you will have to modify the
    "Media browser plus" module.


File: media_browser_plus/media_browser_plus.module
Function: media_browser_plus_move_physical_folder
Line: ~2190


Replace this:

function media_browser_plus_move_physical_folder($source, $destination) {
  $destination = drupal_realpath($destination);
  $source = drupal_realpath($source);
  $jail = drupal_realpath(variable_get('file_default_scheme', 'public') . '://');
  // @todo Please avoid an error by checking the preconditions instead.
  $files = @scandir($source);
  if ($files && count($files) > 2 ) {
    $transfer = new FileTransferLocal($jail);
    $transfer->copyDirectory($source, $destination);
    $transfer->removeDirectory($source);
  }
  else {
    // The folder is empty so just delete and create the new one.
    drupal_rmdir($source);
    file_prepare_directory($destination, FILE_CREATE_DIRECTORY);
  }
}


With this:

function media_browser_plus_move_physical_folder($source, $destination) {
  if (module_exists('eatlas_media_browser_plus_fixes')) {
    eatlas_media_browser_plus_fixes_folder_move($source, $destination);
  } else {
    $destination = drupal_realpath($destination);
    $source = drupal_realpath($source);
    $jail = drupal_realpath(variable_get('file_default_scheme', 'public') . '://');
    // @todo Please avoid an error by checking the preconditions instead.
    $files = @scandir($source);
    if ($files && count($files) > 2 ) {
      $transfer = new FileTransferLocal($jail);
      $transfer->copyDirectory($source, $destination);
      $transfer->removeDirectory($source);
    }
    else {
      // The folder is empty so just delete and create the new one.
      drupal_rmdir($source);
      file_prepare_directory($destination, FILE_CREATE_DIRECTORY);
    }
  }
}

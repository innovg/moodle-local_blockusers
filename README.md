moodle-local_blockusers
=======================
A plugin that allows the site administrator to block multiple users for a certain period by uploading a csv file containing the usernames of the users to be blocked.

Download zip from: https://github.com/coderep/moodle-local_blockusers/tree/master/blockusers

Unzip into the 'local' subfolder of your Moodle install.

Rename the new folder to blockusers.

You are required to add the following lines to moodle/login/index.php, for the plugin blocking mechanism to work:

$username = $frm->username;<br>
	global $DB;<br>
	$dbman = $DB->get_manager();<br>
	$timezone = "Asia/Calcutta"; //set your timezone here<br>
	if(function_exists('date_default_timezone_set')) date_default_timezone_set($timezone);<br>
	if($dbman->table_exists("blockusers"))<br>
	{<br>
		$curTime = time();<br>
		$queryString = "SELECT * FROM mdl_blockusers WHERE start_timestamp <= ".$curTime." && ".$curTime." <= stop_timestamp && username like '".$username."'";<br>
		if($DB->record_exists_sql($queryString))<br>
		{<br>
			$errormsg = "You are not permitted to login";<br>
			$errorcode = 3;<br>
		}<br>
	}<br>
	
Visit http://yoursite.com/admin to finish the installation.

This plugin can be used in situations where you do not want a set of students to take an exam. You can insert their usernames in a csv file,
and upload that file. Set the time for which they are to be blocked.

E.g. of csv file contents: Ron.Smith,Jane.Jacob,John.Doe,Jane.Doe

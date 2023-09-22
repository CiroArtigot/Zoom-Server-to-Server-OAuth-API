# Zoom-Server-to-Server-OAuth-API
This is a PHP code to access the Zoom API with a Server-to-Server-OAuth app

For more info you can contac me on Linked In
https://www.linkedin.com/in/ciroartigot/

 ```php
$accountID = 'xxxxxxxxxxxx';
$clientId = 'xxxxxxxxxxxxx';
$clientSecret = 'xxxxxxxxxxxxxxx';

$authHeader = base64_encode($clientId . ':' . $clientSecret);
$url = 'https://zoom.us/oauth/token';

$data = [
    'grant_type' => 'account_credentials',
    'account_id' => $accountID,
];

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data));
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'Host: zoom.us',
    'Authorization: Basic ' . $authHeader,
]);

$response = curl_exec($ch);

if ($response === false) {
    echo 'Error CURL: ' . curl_error($ch);
} else {
    $responseData = json_decode($response, true);
	$access_token = $responseData['access_token'];
}

curl_close($ch);

if($access_token) {
	$apiCurl = curl_init('https://api.zoom.us/v2/users/me/webinars');
	curl_setopt($apiCurl, CURLOPT_HTTPGET, true);
	curl_setopt($apiCurl, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($apiCurl, CURLOPT_HTTPHEADER, array(
	'Authorization: Bearer ' . $access_token,
	));
	$apiResponse = curl_exec($apiCurl);

	if ($apiResponse === false) echo 'Error: ' . curl_error($apiCurl);
	else echo $apiResponse;
	curl_close($apiCurl);	
}

 ```


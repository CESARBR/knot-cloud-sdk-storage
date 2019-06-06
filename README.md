# knot-cloud-sdk-js-storage

KNoT Cloud storage service JavaScript library

# Quickstart

## Install

```console
npm install --save @cesarbr/knot-cloud-sdk-js-storage
```

## Run

`KNoTCloudStorage` connects to &lt;protocol&gt;://&lt;hostname&gt;:&lt;port&gt; using user or gateway credentials (device owner). Replace this address with your storage instance and the credentials with valid ones.

```javascript
const KNoTCloudStorage = require('@cesarbr/knot-cloud-sdk-js-storage');
const client = new KNoTCloudStorage({
  protocol: 'https',
  hostname: 'data.knot.cloud',
  id: 'b1a1bd58-c3ef-4cb5-82cd-3a2e0b38dd21',
  token: '3185a6c9d64915f6b468ee8043df4af5f08e1933',
});
```

# Methods

## constructor(options)

Creates a new client storage instance so that you can operate on storage.

### Arguments
- `options` **Object** JSON object.
  * `protocol` **String** (Optional) Either `'http'` or `'https'`. Default: `'https'`.
  * `hostname` **String** KNoT Cloud storage instance host name.
  * `port` **Number** (Optional) KNoT Cloud storage instance port. Default: 443.
  * `pathname` **String** (Optional) Path name on the server.
  * `id` **String** Device owner ID.
  * `token` **Number** Device owner token.

### Example

```javascript
const KNoTCloudStorage = require('@cesarbr/knot-cloud-sdk-js-storage');
const client = new KNoTCloudStorage({
  protocol: 'https',
  hostname: 'data.knot.cloud',
  id: 'b1a1bd58-c3ef-4cb5-82cd-3a2e0b38dd21',
  token: '3185a6c9d64915f6b468ee8043df4af5f08e1933',
});
```

## saveData(privateKey, deviceId, data): &lt;Void&gt;

Save device data message to storage. This method can be used to test if the storage service is saving data properly.

### Arguments
  - `privateKey` **String** Base64 private key (it can be the same you've used to deploy the cloud).
  - `deviceId` **String** Device ID (UUID format version).
  - `data` **String** Data to be saved, formed by:
    * `sensorId` **Number** Sensor ID.
    * `value` **String|Boolean|Number** Sensor value. String must be Base64 encoded.

### Example

```javascript
const KNoTCloudStorage = require('@cesarbr/knot-cloud-sdk-js-storage');
const client = new KNoTCloudStorage({
  protocol: 'https',
  hostname: 'data.knot.cloud',
  id: 'b1a1bd58-c3ef-4cb5-82cd-3a2e0b38dd21',
  token: '3185a6c9d64915f6b468ee8043df4af5f08e1933',
});

const privateKey = 'base64-valid-private-key';

client.saveData(privateKey, '3ae370d0-bcba-4aae-86a7-8a2fd2714cef', { sensorId: 1, value: true });
```

## listData(query): &lt;Array&gt;

Get all the device data messages.

### Arguments
- `query` **Object** Optional properties used to filter data.
  * `orderBy` **String** The field used to order.
  * `order` **Number** Ascending (1) or descending (2) order, default=0.
  * `skip` **Number** The number of data to skip (returns skip + 1), default=0.
  * `take` **Number** The maximum number of data that you want from skip + 1 (the number is limited to 100), default=10.
  * `startDate` **String** The start date that you want your set of data (format=YYYY-MM-DD HH:MM:SS).
  * `endDate` **String** The finish date that you want your set of data (format=YYYY-MM-DD HH:MM:SS).

### Result
- `messages` **Array** JSON object containing device data messages.

### Example

```javascript
const KNoTCloudStorage = require('@cesarbr/knot-cloud-sdk-js-storage');
const client = new KNoTCloudStorage({
  protocol: 'https',
  hostname: 'data.knot.cloud',
  id: 'b1a1bd58-c3ef-4cb5-82cd-3a2e0b38dd21',
  token: '3185a6c9d64915f6b468ee8043df4af5f08e1933',
});

client.listData();

// [{
//   from: '188824f0-28c4-475b-ab36-2505402bebcb',
//   payload: {
//       sensorId: 2,
//       value: 234,
//   },
//   timestamp: '2019-03-18T12:48:05.569Z',
// },
// {
//   from: '188824f0-28c4-475b-ab36-2505402bebcb',
//   payload: {
//       sensorId: 1,
//       value: true,
//   },
//   timestamp: '2019-03-18T14:42:03.192Z',
// }]
```

## listDataByDevice(id, query): &lt;Array&gt;

Get the messages sent by a specific device.

### Arguments
- `id` **String** Device ID.
- `query` **Object** Optional properties used to filter data.
  * `orderBy` **String** The field used to order.
  * `order` **Number** Ascending (1) or descending (2) order, default=0.
  * `skip` **Number** The number of data to skip (returns skip + 1), default=0.
  * `take` **Number** The maximum number of data that you want from skip + 1 (the number is limited to 100), default=10.
  * `startDate` **String** The start date that you want your set of data (format=YYYY-MM-DD HH:MM:SS).
  * `endDate` **String** The finish date that you want your set of data (format=YYYY-MM-DD HH:MM:SS).


### Result
- `messages` **Array** JSON object containing device data messages.

### Example

```javascript
const KNoTCloudStorage = require('@cesarbr/knot-cloud-sdk-js-storage');
const client = new KNoTCloudStorage({
  protocol: 'https',
  hostname: 'data.knot.cloud',
  id: 'b1a1bd58-c3ef-4cb5-82cd-3a2e0b38dd21',
  token: '3185a6c9d64915f6b468ee8043df4af5f08e1933',
});

const data = client.listDataById('cc5429a29afcd158');
console.log(data);

// [{
//   from: '188824f0-28c4-475b-ab36-2505402bebcb',
//   payload: {
//       sensorId: 2,
//       value: 234,
//   },
//   timestamp: '2019-03-18T12:48:05.569Z',
// }]
```

## listDataBySensor(deviceId, sensorId, query): &lt;Array&gt;

Get the messages sent by a specific device's sensor.

### Arguments
- `deviceId` **String** Device ID.
- `sensorId` **Number** Sensor ID.
- `query` **Object** Optional properties used to filter data.
  * `orderBy` **String** The field used to order.
  * `order` **Number** Ascending (1) or descending (2) order, default=0.
  * `skip` **Number** The number of data to skip (returns skip + 1), default=0.
  * `take` **Number** The maximum number of data that you want from skip + 1 (the number is limited to 100), default=10.
  * `startDate` **String** The start date that you want your set of data (format=YYYY-MM-DD HH:MM:SS).
  * `endDate` **String** The finish date that you want your set of data (format=YYYY-MM-DD HH:MM:SS).

### Result
- `messages` **Array** JSON object containing device's sensor data messages.

### Example

```javascript
const KNoTCloudStorage = require('@cesarbr/knot-cloud-sdk-js-storage');
const client = new KNoTCloudStorage({
  protocol: 'https',
  hostname: 'data.knot.cloud',
  id: 'b1a1bd58-c3ef-4cb5-82cd-3a2e0b38dd21',
  token: '3185a6c9d64915f6b468ee8043df4af5f08e1933',
});

client.listDataBySensor('cc5429a29afcd158', 1);

// [{
//   from: '188824f0-28c4-475b-ab36-2505402bebcb',
//   payload: {
//       sensorId: 1,
//       value: true,
//   },
//   timestamp: '2019-07-04T02:37:45.365Z',
// }]
```
# Mikhail Medved

* +375 33 387 2292 medvedztel7@gmail.com https://vk.com/imbearmen

* I strive to help people by means of programming. I love not only the result but also the process of working. Ready for challenges

* I can work on 
	* OCC++ 
	* JS 
	* Python

* My latest development (which is being improved): generating content for the Vk group.

```python
import requests
import json
import urllib.request
import time
from time import sleep
import psutil
from clarifai.rest import ClarifaiApp

while True:
	i = 0 
	app = ClarifaiApp(api_key='KEY')
	token = 'token'
	group_id = group_id
	H = time.strftime('%H')
	M = time.strftime('%M')
	pic_number = round((((int(H)*60+int(M))/15)+1))
	flickr_url = 'https://api.flickr.com/services/rest/?method=flickr.interestingness.getList&api_key=KEY&per_page=300&page=1&format=json&nojsoncallback=1'

	def get_relevant_tegs(url): 
		response_data = app.tag_urls([url])
		tag_urls = []
		for  i  in range(5):
			tag_urls.append('#'+ response_data['outputs'][0]['data']['concepts'][i]['name'])
		return tag_urls

	def getFlickrPic():
		pic = requests.get(flickr_url).json()
		#print(json.dumps(pic, indent=4, sort_keys=True))
		getSize = requests.get('https://api.flickr.com/services/rest/?method=flickr.photos.getSizes&api_key=03593667c94923eef10be7a5eca89b13&photo_id='+str(pic['photos']['photo'][pic_number-1]['id'])+'&format=json&nojsoncallback=1').json()
		return (str(getSize['sizes']['size'][8]['source']))

	def getWallUploadServer():
		r = requests.get('https://api.vk.com/method/photos.getWallUploadServer?', params = {'access_token':token,
																				'group_id':group_id,
																				'v':'5.101'}).json() 																															
		return r['response']['upload_url']

	def save_r():
				save_result = requests.get('https://api.vk.com/method/photos.saveWallPhoto?', params ={'access_token':token,
																							'group_id':group_id,
																							'photo':upload_response['photo'],
																							'server':upload_response['server'],
																							'hash':upload_response['hash'],
																							'caption': caption,
																							'v':'5.101'}).json()
				return ('photo'+str(save_result['response'][0]['owner_id'])+'_'+str(save_result['response'][0]['id'])+'&access_key='+str(save_result['response'][0]['access_key']))

	def main():
		url = getFlickrPic()
		img = urllib.request.urlopen(url).read()
		out = open(str(pic_number)+'.jpg', 'wb')
		out.write(img)
		out.close()
		upload_url = getWallUploadServer()
		file = {'file1': open(str(pic_number)+'.jpg', 'rb') }
		global upload_response
		global caption
		caption =' '.join(get_relevant_tegs(url))
		upload_response = requests.post(upload_url, files=file).json()
		save_result = save_r()
		result2 = requests.get('https://api.vk.com/method/wall.post?', params ={'attachments':save_result,
																				'owner_id':-group_id,
																				'access_token':token,
																				'from_group': '1',
																				'message': caption,
																				'v':'5.101'}).json()
		print(pic_number,'=', result2, time.strftime('%H')+':'+time.strftime('%M'))
		sleep(3600)


	if __name__ == '__main__':
		main()
```
* I haven't written anything serious yet

* Studying at Brest state technical University at the faculty of electronic information systems

* I have been studying English since school to this day

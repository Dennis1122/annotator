# annotator
image annotator


def predict_file(request):
	
	cordinates = []
	cords = request.POST.get('cords')
	cords = cords.split(",")
	print("cords--->>>",cords)
	attributes = request.POST.get('attributes')
	attributes = attributes.split(",")
	print("attributes--->>>",attributes)
	values = request.POST.get('values')
	values = values.split(",")
	print("values--->>>",values)
	no_of_boxes = len(cords)/4
	print("boxes-->>",no_of_boxes)

	imagepath="C:/Users/
	Desktop/DB/images/2014/page-001.jpg"
	for i in range(0,int(no_of_boxes)):
		print(i)
		boxCord = cords[i*4:i*4 + 4]
		cordinates.append(boxCord)
	print("cordinates-->>",cordinates)
	word_boxes = getWordBox(imagepath)
	feature = []
	for i in range(0,len(cordinates)):
		df_test=getText(word_boxes,cordinates[i])
		df_new=df_test.sort_values(['x1'],ascending=True)
		text = ' '.join(df_new["value"])
		feature.append(text)
		print("feature--->>",feature)

	saveToCSV(attributes,feature,cordinates)
	# csvData = [][]
	# for i in range(0,int(len(feature)/2)):

	#print("data frame-->>",df_test)
	return HttpResponse('success')
	

//Java script

        $("#submitCords").click(function() {
            var cords = cord.toString();
            var attr = attribute.toString();
            var val = values.toString();
            console.log("rect data-->>",cords);
            //alert(rect);
            $.ajax({
                url: "/Demo/predict_file/",
                type: "POST",
                dataType: "json",
                data: {
                    cords: cords,
                    attributes: attr,
                    values: val,
                    csrfmiddlewaretoken: '{{ csrf_token }}'
                    },
                success : function(json) {
                    alert("Successfully sent the URL to Django");
                },
                error : function(xhr,errmsg,err) {
                    alert("Data sucessfully send and processed" + xhr.status + ": " + xhr.responseText);
                }
            });
        });
    });

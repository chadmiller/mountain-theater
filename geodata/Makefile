$(shell touch --date="5 days ago" freshness-pump)

#range := 47-50,121-126
#range := 46,121
range := 47-49,121-126

all: clean terrain${range}.envi terrain${range}.vrt.js #terrain${range}.tiff #terrain${range}.png

clean:
	rm -fv terrain*.vrt terrain*.png terrain*.envi
	rm -fv terrain${range}.envi terrain${range}.vrt.js
	rm -rfv org?-grd-*

#terrain${range}.tiff:
#	gdalwarp -te {x-min} {y-min} {x-max} {y-max} <path to .vrt> <path to .tif>

terrain${range}.%: terrain${range}.vrt
	gdal_translate -scale 0 2470 0 65535 -outsize 1% 1% -ot UInt16 -of $* $< $@
	#gdal_translate -scale 0 2470 0 65535                 -ot UInt16 -of $* $< $@
	#gdal_translate -scale 0 2470 0 65535 -outsize 1501 901 -ot UInt16 -of $* $< $@

terrain${range}.vrt.js: terrain${range}.vrt
	gdalinfo -nomd $< |grep -Po '(?<=Size\ is\ ).*' |python3 -c 'import json, sys; print("window.map_terrain_grid_size = " + json.dumps(list(int(x)-1 for x in sys.stdin.read().split(", "))) + ";")' |tee $@

terrain46,121.vrt: org1-grd-n46w121
	gdalbuildvrt $@ $(addsuffix /*.flt,$(filter org1-grd-%,$^)) $(addsuffix /*.flt,$(filter org2-grd-%,$^))

terrain47-49,121-122.vrt: org1-grd-n47w121 org1-grd-n48w121
	gdalbuildvrt $@ $(addsuffix /*.flt,$(filter org1-grd-%,$^)) $(addsuffix /*.flt,$(filter org2-grd-%,$^))

terrain47-49,121-126.vrt: org1-grd-n47w121 org2-grd-n47w122 org2-grd-n47w123 org2-grd-n47w124 org1-grd-n47w125
terrain47-49,121-126.vrt: org1-grd-n48w121 org2-grd-n48w122 org2-grd-n48w123 org2-grd-n48w124 org2-grd-n48w125
terrain47-49,121-126.vrt: org2-grd-n49w121 org2-grd-n49w122 org2-grd-n49w123 org2-grd-n49w124 org1-grd-n49w125 
	gdalbuildvrt $@ $(addsuffix /*.flt,$(filter org1-grd-%,$^)) $(addsuffix /*.flt,$(filter org2-grd-%,$^))
	#gdalbuildvrt -allow_projection_difference $@ $(addsuffix /*.flt,$(filter org1-grd-%,$^)) $(addsuffix /*.flt,$(filter org2-grd-%,$^))
	
terrain47-49,121-123.vrt: org1-grd-n47w121 org2-grd-n47w122 org1-grd-n48w121 org2-grd-n48w122 
	gdalbuildvrt $@ $(addsuffix /*.flt,$(filter org1-grd-%,$^)) $(addsuffix /*.flt,$(filter org2-grd-%,$^))
	#gdalbuildvrt -allow_projection_difference $@ $(addsuffix /*.flt,$(filter org1-grd-%,$^)) $(addsuffix /*.flt,$(filter org2-grd-%,$^))
	
org1-grd-%: org1-zip-%
	rm -rf $@
	mkdir -p $@
	unzip -d $@ -L $< float\*

org2-grd-%: org2-zip-%
	rm -rf $@
	mkdir -p $@
	unzip -d $@ -L -C $<

org2-zip-%: freshness-pump
	-cp $@ $@-tmp
	wget -c -T 5 -O $@-tmp https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/GridFloat/USGS_NED_1_$*_GridFloat.zip
	mv $@-tmp $@

org1-zip-%: freshness-pump
	-cp $@ $@-tmp
	wget -c -T 5 -O $@-tmp https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/GridFloat/$*.zip
	mv $@-tmp $@

freshness-pump:
	touch $@

.PRECIOUS: org1-zip-% org2-zip-%


# 46.5 - 47.99999,  121.0 - 123.9999
#https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/USGS_NED_1_n47w122_ArcGrid.zip
#https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/USGS_NED_1_n47w123_ArcGrid.zip
#https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/USGS_NED_1_n47w124_ArcGrid.zip
#https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/USGS_NED_1_n48w122_ArcGrid.zip
#https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/USGS_NED_1_n48w123_ArcGrid.zip
#https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/USGS_NED_1_n48w124_ArcGrid.zip
#https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/USGS_NED_1_n48w125_ArcGrid.zip
#https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/USGS_NED_1_n49w122_ArcGrid.zip
#https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/USGS_NED_1_n49w123_ArcGrid.zip
#https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/USGS_NED_1_n49w124_ArcGrid.zip
#https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/n47w125.zip
#https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/n49w124.zip
#https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/n49w125.zip

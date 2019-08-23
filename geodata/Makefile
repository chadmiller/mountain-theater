$(shell touch --date="5 days ago" freshness-pump)

range := n46-n49,w126-w121

all: clean terrain-${range}.envi terrain-${range}.vrt

clean:
	rm -fv terrain-*.vrt terrain-*.envi
	rm -fv terrain-${range}.envi
	rm -rfv org?-grd-*

terrain-${range}.%: MIN_ALTITUDE := 0
terrain-${range}.%: MAX_ALTITUDE := 2470
terrain-${range}.%: RESOLUTION := 100
terrain-${range}.%: terrain-${range}.vrt
	rm -f $@.aux.xml
	gdal_translate -epo -strict -scale ${MIN_ALTITUDE} ${MAX_ALTITUDE} 0 65535 -outsize 2% 0 -ot UInt16 -of $* $< $@
	#gdal_translate -scale ${MIN_ALTITUDE} ${MAX_ALTITUDE} 0 65535 -tr ${RESOLUTION} ${RESOLUTION} -ot UInt16 -of $* $< $@-usingres || true

#terrain-n46-n49,w126-w121.vrt: org1-grd-n46w121 org2-grd-n46w122 org2-grd-n46w123 org1-grd-n46w124 org1-grd-n46w125
terrain-n46-n49,w126-w121.vrt: org1-grd-n47w121 org2-grd-n47w122 org2-grd-n47w123 org2-grd-n47w124 org1-grd-n47w125
terrain-n46-n49,w126-w121.vrt: org1-grd-n48w121 org2-grd-n48w122 org2-grd-n48w123 org2-grd-n48w124 org2-grd-n48w125
terrain-n46-n49,w126-w121.vrt: org2-grd-n49w121 org2-grd-n49w122 org2-grd-n49w123 org2-grd-n49w124 org1-grd-n49w125 
terrain-n46-n49,w126-w121.vrt:
	
	set -x; gdalbuildvrt -overwrite $@ $(addsuffix /*.flt,$(filter org1-grd-%,$^)) $(addsuffix /*.flt,$(filter org2-grd-%,$^))
	
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

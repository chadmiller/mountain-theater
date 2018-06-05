NORTH := 49
WEST := 125

terrain.tiff: grand.vrt
	gdalwarp -t_srs EPSG:32632 -r bilinear $< $@

grand.vrt: $(shell seq $$((${NORTH}-1)) $$((${NORTH}+1)) |while read n; do seq $$((${WEST}-1)) $$((${WEST}+1)) |while read w; do echo grdn$${n}w$${w}_1; done; done)
	gdalbuildvrt $@ $^
	gdalinfo $<
	gdalinfo $@


#grd%_1.png: SCALE := 111120
#grd%_1.png: grd%_1
#	gdalinfo $<
#	gdalmanage copy $< $@
#	test -f $@
#	#gdaldem TPI $< $@ -of xyz -scale ${SCALE}

grd%_1: %.zip
	unzip $< $@/\*
	gdalinfo $@
	
%.zip: 
	#wget -nv -c https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/1/ArcGrid/$*.zip


USGS_NED_13_%_GridFloat.zip:
	#wget -nv -c https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/13/GridFloat/USGS_NED_13_$*_GridFloat.zip
	wget -nv -c https://prd-tnm.s3.amazonaws.com/StagedProducts/Elevation/13/GridFloat/$@

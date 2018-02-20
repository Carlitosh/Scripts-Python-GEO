#!/usr/bin/python
# -*- coding: utf-8 -*-
import gdal
from mayavi import mlab
import numpy as np

# 1) abriendo el DEM geotiff como un  array
path = 'dem_corregido.tif'

DEM = gdal.Open(path)
dem_cordoba = DEM.ReadAsArray()

# 2) transformacion de coordenadas
columns = DEM.RasterXSize
rows = DEM.RasterYSize
gt = DEM.GetGeoTransform()
ndv = DEM.GetRasterBand(1).GetNoDataValue()

x = (columns * gt[1]) + gt[0]
y = (rows * gt[5]) + gt[3]

X = np.arange(gt[0], x, gt[1])
Y = np.arange(gt[3], y, gt[5])

# 3) creacion de una simple malla sin interpolacion
X, Y = np.meshgrid(X, Y)

#Mayavi requiere un orden columna,fila. GDAL lee en orden filas,columnas  (i.e y, x) 
dem_cordoba = np.rollaxis(dem_cordoba,0,2)
X = np.rollaxis(X,0,2)
Y = np.rollaxis(Y,0,2)

# 4) Borrar los valores sin datos
dem_cordoba = dem_cordoba.astype(np.float32)
dem_cordoba[dem_cordoba == ndv] = np.nan #if it's NaN, mayavi will interpolate

# Borrar la ultima columna
dem_cordoba = np.delete(dem_cordoba, len(dem_cordoba)-1, axis = 0)
X = np.delete(X, len(X)-1, axis = 0)
Y = np.delete(Y, len(Y)-1, axis = 0)

# Borrar la ultima fila
dem_cordoba = np.delete(dem_cordoba, len(dem_cordoba[0])-1, axis = 1)
X = np.delete(X, len(X[0])-1, axis = 1)
Y = np.delete(Y, len(Y[0])-1, axis = 1)



# creando la figura y el render con mayavi
mlab.figure(1, size=(500, 250), fgcolor=(1, 1, 1),
                                    bgcolor=(0.5, 0.5, 0.5))

surf = mlab.surf(X, Y, dem_cordoba, warp_scale="auto")

# colocar en el render la barra de colores
mlab.colorbar()
mlab.show()

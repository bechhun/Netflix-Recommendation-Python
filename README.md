# Netflix-Recommendation-Python
{
 "cells": [
  {
   "cell_type": "markdown",
   "id": "a6d835be",
   "metadata": {},
   "source": [
    "# Netflix Recommendation Project\n",
    "##### _By Bora Chhun_"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "7fbe2a7b",
   "metadata": {},
   "source": [
    "### _Import packages_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "id": "224728c9",
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import numpy as np\n",
    "import pandas as pd\n",
    "import matplotlib.pyplot as plt\n",
    "import seaborn as sns\n",
    "from sklearn import svm"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "6c89a6c0",
   "metadata": {},
   "source": [
    "### _Import Data_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "id": "75a3229c",
   "metadata": {},
   "outputs": [],
   "source": [
    "movies = pd.read_csv(r'C:\\Users\\Bora\\Management Of RBR\\RBR Builds - Documents\\RBR Analytics\\Analytics Portfolio\\Net Recommendation Project\\movies.csv', encoding = 'latin1')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "id": "3306ccdd",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>movieId</th>\n",
       "      <th>title</th>\n",
       "      <th>genres</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>Toy Story (1995)</td>\n",
       "      <td>Adventure|Animation|Children|Comedy|Fantasy</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>Jumanji (1995)</td>\n",
       "      <td>Adventure|Children|Fantasy</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>Grumpier Old Men (1995)</td>\n",
       "      <td>Comedy|Romance</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4</td>\n",
       "      <td>Waiting to Exhale (1995)</td>\n",
       "      <td>Comedy|Drama|Romance</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>5</td>\n",
       "      <td>Father of the Bride Part II (1995)</td>\n",
       "      <td>Comedy</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>62418</th>\n",
       "      <td>209157</td>\n",
       "      <td>We (2018)</td>\n",
       "      <td>Drama</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>62419</th>\n",
       "      <td>209159</td>\n",
       "      <td>Window of the Soul (2001)</td>\n",
       "      <td>Documentary</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>62420</th>\n",
       "      <td>209163</td>\n",
       "      <td>Bad Poems (2018)</td>\n",
       "      <td>Comedy|Drama</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>62421</th>\n",
       "      <td>209169</td>\n",
       "      <td>A Girl Thing (2001)</td>\n",
       "      <td>(no genres listed)</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>62422</th>\n",
       "      <td>209171</td>\n",
       "      <td>Women of Devil's Island (1962)</td>\n",
       "      <td>Action|Adventure|Drama</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>62423 rows × 3 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "       movieId                               title   \n",
       "0            1                    Toy Story (1995)  \\\n",
       "1            2                      Jumanji (1995)   \n",
       "2            3             Grumpier Old Men (1995)   \n",
       "3            4            Waiting to Exhale (1995)   \n",
       "4            5  Father of the Bride Part II (1995)   \n",
       "...        ...                                 ...   \n",
       "62418   209157                           We (2018)   \n",
       "62419   209159           Window of the Soul (2001)   \n",
       "62420   209163                    Bad Poems (2018)   \n",
       "62421   209169                 A Girl Thing (2001)   \n",
       "62422   209171      Women of Devil's Island (1962)   \n",
       "\n",
       "                                            genres  \n",
       "0      Adventure|Animation|Children|Comedy|Fantasy  \n",
       "1                       Adventure|Children|Fantasy  \n",
       "2                                   Comedy|Romance  \n",
       "3                             Comedy|Drama|Romance  \n",
       "4                                           Comedy  \n",
       "...                                            ...  \n",
       "62418                                        Drama  \n",
       "62419                                  Documentary  \n",
       "62420                                 Comedy|Drama  \n",
       "62421                           (no genres listed)  \n",
       "62422                       Action|Adventure|Drama  \n",
       "\n",
       "[62423 rows x 3 columns]"
      ]
     },
     "execution_count": 20,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "movies"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "d45b925f",
   "metadata": {},
   "source": [
    "### _Cleaning Movie Titles with REGEX_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "id": "e1ffbe97",
   "metadata": {},
   "outputs": [],
   "source": [
    "import re\n",
    "\n",
    "def clean_title(title):\n",
    "    return re.sub(\"[^a-zA-Z0-9 ]\", \"\", title)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "id": "c0e27915",
   "metadata": {},
   "outputs": [],
   "source": [
    "movies[\"clean_title\"] = movies[\"title\"].apply(clean_title)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "id": "1f46e120",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>movieId</th>\n",
       "      <th>title</th>\n",
       "      <th>genres</th>\n",
       "      <th>clean_title</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>Toy Story (1995)</td>\n",
       "      <td>Adventure|Animation|Children|Comedy|Fantasy</td>\n",
       "      <td>Toy Story 1995</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>Jumanji (1995)</td>\n",
       "      <td>Adventure|Children|Fantasy</td>\n",
       "      <td>Jumanji 1995</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>Grumpier Old Men (1995)</td>\n",
       "      <td>Comedy|Romance</td>\n",
       "      <td>Grumpier Old Men 1995</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4</td>\n",
       "      <td>Waiting to Exhale (1995)</td>\n",
       "      <td>Comedy|Drama|Romance</td>\n",
       "      <td>Waiting to Exhale 1995</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>5</td>\n",
       "      <td>Father of the Bride Part II (1995)</td>\n",
       "      <td>Comedy</td>\n",
       "      <td>Father of the Bride Part II 1995</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>62418</th>\n",
       "      <td>209157</td>\n",
       "      <td>We (2018)</td>\n",
       "      <td>Drama</td>\n",
       "      <td>We 2018</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>62419</th>\n",
       "      <td>209159</td>\n",
       "      <td>Window of the Soul (2001)</td>\n",
       "      <td>Documentary</td>\n",
       "      <td>Window of the Soul 2001</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>62420</th>\n",
       "      <td>209163</td>\n",
       "      <td>Bad Poems (2018)</td>\n",
       "      <td>Comedy|Drama</td>\n",
       "      <td>Bad Poems 2018</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>62421</th>\n",
       "      <td>209169</td>\n",
       "      <td>A Girl Thing (2001)</td>\n",
       "      <td>(no genres listed)</td>\n",
       "      <td>A Girl Thing 2001</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>62422</th>\n",
       "      <td>209171</td>\n",
       "      <td>Women of Devil's Island (1962)</td>\n",
       "      <td>Action|Adventure|Drama</td>\n",
       "      <td>Women of Devils Island 1962</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>62423 rows × 4 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "       movieId                               title   \n",
       "0            1                    Toy Story (1995)  \\\n",
       "1            2                      Jumanji (1995)   \n",
       "2            3             Grumpier Old Men (1995)   \n",
       "3            4            Waiting to Exhale (1995)   \n",
       "4            5  Father of the Bride Part II (1995)   \n",
       "...        ...                                 ...   \n",
       "62418   209157                           We (2018)   \n",
       "62419   209159           Window of the Soul (2001)   \n",
       "62420   209163                    Bad Poems (2018)   \n",
       "62421   209169                 A Girl Thing (2001)   \n",
       "62422   209171      Women of Devil's Island (1962)   \n",
       "\n",
       "                                            genres   \n",
       "0      Adventure|Animation|Children|Comedy|Fantasy  \\\n",
       "1                       Adventure|Children|Fantasy   \n",
       "2                                   Comedy|Romance   \n",
       "3                             Comedy|Drama|Romance   \n",
       "4                                           Comedy   \n",
       "...                                            ...   \n",
       "62418                                        Drama   \n",
       "62419                                  Documentary   \n",
       "62420                                 Comedy|Drama   \n",
       "62421                           (no genres listed)   \n",
       "62422                       Action|Adventure|Drama   \n",
       "\n",
       "                            clean_title  \n",
       "0                        Toy Story 1995  \n",
       "1                          Jumanji 1995  \n",
       "2                 Grumpier Old Men 1995  \n",
       "3                Waiting to Exhale 1995  \n",
       "4      Father of the Bride Part II 1995  \n",
       "...                                 ...  \n",
       "62418                           We 2018  \n",
       "62419           Window of the Soul 2001  \n",
       "62420                    Bad Poems 2018  \n",
       "62421                 A Girl Thing 2001  \n",
       "62422       Women of Devils Island 1962  \n",
       "\n",
       "[62423 rows x 4 columns]"
      ]
     },
     "execution_count": 23,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "movies"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "3f644386",
   "metadata": {},
   "source": [
    "### _Creating a TFIDF Matrix_\n",
    "##### Term Frequency"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "id": "e994ed4b",
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.feature_extraction.text import TfidfVectorizer\n",
    "\n",
    "vectorizer = TfidfVectorizer(ngram_range=(1,2))\n",
    "\n",
    "tfidf = vectorizer.fit_transform(movies[\"clean_title\"])"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "09bb84c7",
   "metadata": {},
   "source": [
    "### _Creating a Search Function_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 63,
   "id": "2758be24",
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.metrics.pairwise import cosine_similarity\n",
    "import numpy as np\n",
    "\n",
    "def search(title):\n",
    "    title = clean_title(title)\n",
    "    query_vec = vectorizer.transform([title])\n",
    "    similarity = cosine_similarity(query_vec, tfidf).flatten()\n",
    "    indices = np.argpartition(similarity, -5) [-5:]\n",
    "    results = movies.iloc[indices][::-1]\n",
    "    return results"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "63bf9d84",
   "metadata": {},
   "source": [
    "### _Building an Interactive Search Box with Jupyter_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 66,
   "id": "6e5f45d8",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.jupyter.widget-view+json": {
       "model_id": "00bed620ce0843d7aad81da88896a7c1",
       "version_major": 2,
       "version_minor": 0
      },
      "text/plain": [
       "Text(value='Toy Story', description='Movie Title:')"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "application/vnd.jupyter.widget-view+json": {
       "model_id": "da5910056f524e6ab29e6f57d13908cd",
       "version_major": 2,
       "version_minor": 0
      },
      "text/plain": [
       "Output()"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "import ipywidgets as widgets\n",
    "from IPython.display import display\n",
    "\n",
    "movie_input = widgets.Text(\n",
    "value=\"Toy Story\",\n",
    "description=\"Movie Title:\",\n",
    "disable= False\n",
    ")\n",
    "movie_list = widgets.Output()\n",
    "\n",
    "def on_type(data):\n",
    "    with movie_list:\n",
    "        movie_list.clear_output()\n",
    "        title = data[\"new\"]\n",
    "        if len(title) > 5:\n",
    "            display(search(title))\n",
    "\n",
    "movie_input.observe(on_type, names='value')\n",
    "\n",
    "display(movie_input, movie_list)"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "94fe3d4c",
   "metadata": {},
   "source": [
    "### _Reading in Movie Ratings Data_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 67,
   "id": "c217a091",
   "metadata": {},
   "outputs": [],
   "source": [
    "ratings = pd.read_csv(r'C:\\Users\\Bora\\Management Of RBR\\RBR Builds - Documents\\RBR Analytics\\Analytics Portfolio\\Net Recommendation Project\\ratings.csv', encoding = 'latin1')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 153,
   "id": "03d28bbf",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>userId</th>\n",
       "      <th>movieId</th>\n",
       "      <th>rating</th>\n",
       "      <th>timestamp</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>296</td>\n",
       "      <td>5.0</td>\n",
       "      <td>1147880044</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1</td>\n",
       "      <td>306</td>\n",
       "      <td>3.5</td>\n",
       "      <td>1147868817</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>1</td>\n",
       "      <td>307</td>\n",
       "      <td>5.0</td>\n",
       "      <td>1147868828</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>1</td>\n",
       "      <td>665</td>\n",
       "      <td>5.0</td>\n",
       "      <td>1147878820</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>1</td>\n",
       "      <td>899</td>\n",
       "      <td>3.5</td>\n",
       "      <td>1147868510</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25000090</th>\n",
       "      <td>162541</td>\n",
       "      <td>50872</td>\n",
       "      <td>4.5</td>\n",
       "      <td>1240953372</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25000091</th>\n",
       "      <td>162541</td>\n",
       "      <td>55768</td>\n",
       "      <td>2.5</td>\n",
       "      <td>1240951998</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25000092</th>\n",
       "      <td>162541</td>\n",
       "      <td>56176</td>\n",
       "      <td>2.0</td>\n",
       "      <td>1240950697</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25000093</th>\n",
       "      <td>162541</td>\n",
       "      <td>58559</td>\n",
       "      <td>4.0</td>\n",
       "      <td>1240953434</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25000094</th>\n",
       "      <td>162541</td>\n",
       "      <td>63876</td>\n",
       "      <td>5.0</td>\n",
       "      <td>1240952515</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>25000095 rows × 4 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "          userId  movieId  rating   timestamp\n",
       "0              1      296     5.0  1147880044\n",
       "1              1      306     3.5  1147868817\n",
       "2              1      307     5.0  1147868828\n",
       "3              1      665     5.0  1147878820\n",
       "4              1      899     3.5  1147868510\n",
       "...          ...      ...     ...         ...\n",
       "25000090  162541    50872     4.5  1240953372\n",
       "25000091  162541    55768     2.5  1240951998\n",
       "25000092  162541    56176     2.0  1240950697\n",
       "25000093  162541    58559     4.0  1240953434\n",
       "25000094  162541    63876     5.0  1240952515\n",
       "\n",
       "[25000095 rows x 4 columns]"
      ]
     },
     "execution_count": 153,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "ratings"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 154,
   "id": "4a900191",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "userId         int64\n",
       "movieId        int64\n",
       "rating       float64\n",
       "timestamp      int64\n",
       "dtype: object"
      ]
     },
     "execution_count": 154,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "ratings.dtypes"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "f48aaf66",
   "metadata": {},
   "source": [
    "### _Finding Users Who Liked the Same Movie as Us_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 155,
   "id": "f8be1485",
   "metadata": {},
   "outputs": [],
   "source": [
    "movie_id = 1"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 156,
   "id": "96c3aeb0",
   "metadata": {},
   "outputs": [],
   "source": [
    "\n",
    "similar_users = ratings[(ratings[\"movieId\"] == movie_id) & (ratings[\"rating\"] > 4)][\"userId\"].unique()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 157,
   "id": "eeb4ae9b",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([    36,     75,     86, ..., 162527, 162530, 162533], dtype=int64)"
      ]
     },
     "execution_count": 157,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "similar_users"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 158,
   "id": "124e26f5",
   "metadata": {},
   "outputs": [],
   "source": [
    "similar_user_recs = ratings[(ratings[\"userId\"].isin(similar_users)) & (ratings[\"rating\"] > 4)][\"movieId\"]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 159,
   "id": "ce3195dc",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "5101            1\n",
       "5105           34\n",
       "5111          110\n",
       "5114          150\n",
       "5127          260\n",
       "            ...  \n",
       "24998854    60069\n",
       "24998861    67997\n",
       "24998876    78499\n",
       "24998884    81591\n",
       "24998888    88129\n",
       "Name: movieId, Length: 1358326, dtype: int64"
      ]
     },
     "execution_count": 159,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "similar_user_recs    "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 160,
   "id": "66b16e10",
   "metadata": {},
   "outputs": [],
   "source": [
    "similar_user_recs = similar_user_recs.value_counts() / len(similar_users)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 161,
   "id": "c42132de",
   "metadata": {},
   "outputs": [],
   "source": [
    "similar_user_recs = similar_user_recs[similar_user_recs >.1]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 162,
   "id": "4f91f401",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "movieId\n",
       "1        1.000000\n",
       "318      0.445607\n",
       "260      0.403770\n",
       "356      0.370215\n",
       "296      0.367295\n",
       "           ...   \n",
       "953      0.103053\n",
       "551      0.101195\n",
       "1222     0.100876\n",
       "745      0.100345\n",
       "48780    0.100186\n",
       "Name: count, Length: 113, dtype: float64"
      ]
     },
     "execution_count": 162,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "similar_user_recs"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "42697e2d",
   "metadata": {},
   "source": [
    "### _Finding How Much All Users Like Movies_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 171,
   "id": "cbe1c2c2",
   "metadata": {},
   "outputs": [],
   "source": [
    "all_users = ratings[(ratings[\"movieId\"].isin(similar_user_recs.index)) & (ratings[\"rating\"] > 4)]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 172,
   "id": "526c900d",
   "metadata": {},
   "outputs": [],
   "source": [
    "all_users_recs = all_users[\"movieId\"].value_counts() / len(all_users[\"userId\"].unique())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 174,
   "id": "45afd45f",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "movieId\n",
       "318      0.342220\n",
       "296      0.284674\n",
       "2571     0.244033\n",
       "356      0.235266\n",
       "593      0.225909\n",
       "           ...   \n",
       "551      0.040918\n",
       "50872    0.039111\n",
       "745      0.037031\n",
       "78499    0.035131\n",
       "2355     0.025091\n",
       "Name: count, Length: 113, dtype: float64"
      ]
     },
     "execution_count": 174,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "all_users_recs"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "04098cf3",
   "metadata": {},
   "source": [
    "### _Creating a Recommendation Score_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 176,
   "id": "439be04a",
   "metadata": {},
   "outputs": [],
   "source": [
    "rec_percentages = pd.concat([similar_user_recs, all_users_recs], axis = 1)\n",
    "rec_percentages.columns = [\"similar\", \"all\"]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 177,
   "id": "ab0bb055",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>similar</th>\n",
       "      <th>all</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>movieId</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.124728</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>318</th>\n",
       "      <td>0.445607</td>\n",
       "      <td>0.342220</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>260</th>\n",
       "      <td>0.403770</td>\n",
       "      <td>0.222207</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>356</th>\n",
       "      <td>0.370215</td>\n",
       "      <td>0.235266</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>296</th>\n",
       "      <td>0.367295</td>\n",
       "      <td>0.284674</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>953</th>\n",
       "      <td>0.103053</td>\n",
       "      <td>0.045792</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>551</th>\n",
       "      <td>0.101195</td>\n",
       "      <td>0.040918</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1222</th>\n",
       "      <td>0.100876</td>\n",
       "      <td>0.066877</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>745</th>\n",
       "      <td>0.100345</td>\n",
       "      <td>0.037031</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>48780</th>\n",
       "      <td>0.100186</td>\n",
       "      <td>0.068314</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>113 rows × 2 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "          similar       all\n",
       "movieId                    \n",
       "1        1.000000  0.124728\n",
       "318      0.445607  0.342220\n",
       "260      0.403770  0.222207\n",
       "356      0.370215  0.235266\n",
       "296      0.367295  0.284674\n",
       "...           ...       ...\n",
       "953      0.103053  0.045792\n",
       "551      0.101195  0.040918\n",
       "1222     0.100876  0.066877\n",
       "745      0.100345  0.037031\n",
       "48780    0.100186  0.068314\n",
       "\n",
       "[113 rows x 2 columns]"
      ]
     },
     "execution_count": 177,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "rec_percentages"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 180,
   "id": "49c831f9",
   "metadata": {},
   "outputs": [],
   "source": [
    "rec_percentages[\"score\"] = rec_percentages[\"similar\"] / rec_percentages[\"all\"]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 182,
   "id": "01e2250a",
   "metadata": {},
   "outputs": [],
   "source": [
    "rec_percentages = rec_percentages.sort_values(\"score\", ascending=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 183,
   "id": "eabe7cb1",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>similar</th>\n",
       "      <th>all</th>\n",
       "      <th>score</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>movieId</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.124728</td>\n",
       "      <td>8.017414</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3114</th>\n",
       "      <td>0.280648</td>\n",
       "      <td>0.053706</td>\n",
       "      <td>5.225654</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2355</th>\n",
       "      <td>0.110539</td>\n",
       "      <td>0.025091</td>\n",
       "      <td>4.405452</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>78499</th>\n",
       "      <td>0.152960</td>\n",
       "      <td>0.035131</td>\n",
       "      <td>4.354038</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4886</th>\n",
       "      <td>0.235147</td>\n",
       "      <td>0.070811</td>\n",
       "      <td>3.320783</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2858</th>\n",
       "      <td>0.216724</td>\n",
       "      <td>0.167634</td>\n",
       "      <td>1.292845</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>296</th>\n",
       "      <td>0.367295</td>\n",
       "      <td>0.284674</td>\n",
       "      <td>1.290232</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>79132</th>\n",
       "      <td>0.166817</td>\n",
       "      <td>0.131384</td>\n",
       "      <td>1.269693</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4973</th>\n",
       "      <td>0.142501</td>\n",
       "      <td>0.112405</td>\n",
       "      <td>1.267747</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2959</th>\n",
       "      <td>0.262649</td>\n",
       "      <td>0.216717</td>\n",
       "      <td>1.211946</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>113 rows × 3 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "          similar       all     score\n",
       "movieId                              \n",
       "1        1.000000  0.124728  8.017414\n",
       "3114     0.280648  0.053706  5.225654\n",
       "2355     0.110539  0.025091  4.405452\n",
       "78499    0.152960  0.035131  4.354038\n",
       "4886     0.235147  0.070811  3.320783\n",
       "...           ...       ...       ...\n",
       "2858     0.216724  0.167634  1.292845\n",
       "296      0.367295  0.284674  1.290232\n",
       "79132    0.166817  0.131384  1.269693\n",
       "4973     0.142501  0.112405  1.267747\n",
       "2959     0.262649  0.216717  1.211946\n",
       "\n",
       "[113 rows x 3 columns]"
      ]
     },
     "execution_count": 183,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "rec_percentages"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 185,
   "id": "df0d9fee",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>similar</th>\n",
       "      <th>all</th>\n",
       "      <th>score</th>\n",
       "      <th>movieId</th>\n",
       "      <th>title</th>\n",
       "      <th>genres</th>\n",
       "      <th>clean_title</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.124728</td>\n",
       "      <td>8.017414</td>\n",
       "      <td>1</td>\n",
       "      <td>Toy Story (1995)</td>\n",
       "      <td>Adventure|Animation|Children|Comedy|Fantasy</td>\n",
       "      <td>Toy Story 1995</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3021</th>\n",
       "      <td>0.280648</td>\n",
       "      <td>0.053706</td>\n",
       "      <td>5.225654</td>\n",
       "      <td>3114</td>\n",
       "      <td>Toy Story 2 (1999)</td>\n",
       "      <td>Adventure|Animation|Children|Comedy|Fantasy</td>\n",
       "      <td>Toy Story 2 1999</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2264</th>\n",
       "      <td>0.110539</td>\n",
       "      <td>0.025091</td>\n",
       "      <td>4.405452</td>\n",
       "      <td>2355</td>\n",
       "      <td>Bug's Life, A (1998)</td>\n",
       "      <td>Adventure|Animation|Children|Comedy</td>\n",
       "      <td>Bugs Life A 1998</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>14813</th>\n",
       "      <td>0.152960</td>\n",
       "      <td>0.035131</td>\n",
       "      <td>4.354038</td>\n",
       "      <td>78499</td>\n",
       "      <td>Toy Story 3 (2010)</td>\n",
       "      <td>Adventure|Animation|Children|Comedy|Fantasy|IMAX</td>\n",
       "      <td>Toy Story 3 2010</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4780</th>\n",
       "      <td>0.235147</td>\n",
       "      <td>0.070811</td>\n",
       "      <td>3.320783</td>\n",
       "      <td>4886</td>\n",
       "      <td>Monsters, Inc. (2001)</td>\n",
       "      <td>Adventure|Animation|Children|Comedy|Fantasy</td>\n",
       "      <td>Monsters Inc 2001</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>580</th>\n",
       "      <td>0.216618</td>\n",
       "      <td>0.067513</td>\n",
       "      <td>3.208539</td>\n",
       "      <td>588</td>\n",
       "      <td>Aladdin (1992)</td>\n",
       "      <td>Adventure|Animation|Children|Comedy|Musical</td>\n",
       "      <td>Aladdin 1992</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6258</th>\n",
       "      <td>0.228139</td>\n",
       "      <td>0.072268</td>\n",
       "      <td>3.156862</td>\n",
       "      <td>6377</td>\n",
       "      <td>Finding Nemo (2003)</td>\n",
       "      <td>Adventure|Animation|Children|Comedy</td>\n",
       "      <td>Finding Nemo 2003</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>587</th>\n",
       "      <td>0.179400</td>\n",
       "      <td>0.059977</td>\n",
       "      <td>2.991150</td>\n",
       "      <td>595</td>\n",
       "      <td>Beauty and the Beast (1991)</td>\n",
       "      <td>Animation|Children|Fantasy|Musical|Romance|IMAX</td>\n",
       "      <td>Beauty and the Beast 1991</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8246</th>\n",
       "      <td>0.203504</td>\n",
       "      <td>0.068453</td>\n",
       "      <td>2.972889</td>\n",
       "      <td>8961</td>\n",
       "      <td>Incredibles, The (2004)</td>\n",
       "      <td>Action|Adventure|Animation|Children|Comedy</td>\n",
       "      <td>Incredibles The 2004</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>359</th>\n",
       "      <td>0.253411</td>\n",
       "      <td>0.085764</td>\n",
       "      <td>2.954762</td>\n",
       "      <td>364</td>\n",
       "      <td>Lion King, The (1994)</td>\n",
       "      <td>Adventure|Animation|Children|Drama|Musical|IMAX</td>\n",
       "      <td>Lion King The 1994</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "        similar       all     score  movieId                        title   \n",
       "0      1.000000  0.124728  8.017414        1             Toy Story (1995)  \\\n",
       "3021   0.280648  0.053706  5.225654     3114           Toy Story 2 (1999)   \n",
       "2264   0.110539  0.025091  4.405452     2355         Bug's Life, A (1998)   \n",
       "14813  0.152960  0.035131  4.354038    78499           Toy Story 3 (2010)   \n",
       "4780   0.235147  0.070811  3.320783     4886        Monsters, Inc. (2001)   \n",
       "580    0.216618  0.067513  3.208539      588               Aladdin (1992)   \n",
       "6258   0.228139  0.072268  3.156862     6377          Finding Nemo (2003)   \n",
       "587    0.179400  0.059977  2.991150      595  Beauty and the Beast (1991)   \n",
       "8246   0.203504  0.068453  2.972889     8961      Incredibles, The (2004)   \n",
       "359    0.253411  0.085764  2.954762      364        Lion King, The (1994)   \n",
       "\n",
       "                                                 genres   \n",
       "0           Adventure|Animation|Children|Comedy|Fantasy  \\\n",
       "3021        Adventure|Animation|Children|Comedy|Fantasy   \n",
       "2264                Adventure|Animation|Children|Comedy   \n",
       "14813  Adventure|Animation|Children|Comedy|Fantasy|IMAX   \n",
       "4780        Adventure|Animation|Children|Comedy|Fantasy   \n",
       "580         Adventure|Animation|Children|Comedy|Musical   \n",
       "6258                Adventure|Animation|Children|Comedy   \n",
       "587     Animation|Children|Fantasy|Musical|Romance|IMAX   \n",
       "8246         Action|Adventure|Animation|Children|Comedy   \n",
       "359     Adventure|Animation|Children|Drama|Musical|IMAX   \n",
       "\n",
       "                     clean_title  \n",
       "0                 Toy Story 1995  \n",
       "3021            Toy Story 2 1999  \n",
       "2264            Bugs Life A 1998  \n",
       "14813           Toy Story 3 2010  \n",
       "4780           Monsters Inc 2001  \n",
       "580                 Aladdin 1992  \n",
       "6258           Finding Nemo 2003  \n",
       "587    Beauty and the Beast 1991  \n",
       "8246        Incredibles The 2004  \n",
       "359           Lion King The 1994  "
      ]
     },
     "execution_count": 185,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "rec_percentages.head(10).merge(movies, left_index=True, right_on=\"movieId\")"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "ec9bea4d",
   "metadata": {},
   "source": [
    "### _Building a Recommendation Function_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 191,
   "id": "cbca2d0e",
   "metadata": {},
   "outputs": [],
   "source": [
    "def find_similar_movies(movie_id):\n",
    "    similar_users = ratings[(ratings[\"movieId\"] == movie_id) & (ratings[\"rating\"] > 4)][\"userId\"].unique()\n",
    "    similar_user_recs = ratings[(ratings[\"userId\"].isin(similar_users)) & (ratings[\"rating\"] > 4)][\"movieId\"]\n",
    "    \n",
    "    similar_user_recs = similar_user_recs.value_counts() / len(similar_users)\n",
    "    similar_user_recs = similar_user_recs[similar_user_recs >.1]\n",
    "    \n",
    "    all_users = ratings[(ratings[\"movieId\"].isin(similar_user_recs.index)) & (ratings[\"rating\"] > 4)]\n",
    "    all_users_recs = all_users[\"movieId\"].value_counts() / len(all_users[\"userId\"].unique())\n",
    "    \n",
    "    rec_percentages = pd.concat([similar_user_recs, all_users_recs], axis = 1)\n",
    "    rec_percentages.columns = [\"similar\", \"all\"]\n",
    "    \n",
    "    rec_percentages[\"score\"] = rec_percentages[\"similar\"] / rec_percentages[\"all\"]\n",
    "    \n",
    "    rec_percentages = rec_percentages.sort_values(\"score\", ascending=False)\n",
    "    return rec_percentages.head(10).merge(movies, left_index=True, right_on=\"movieId\") [[\"score\", \"title\", \"genres\"]]\n",
    "    \n",
    "    "
   ]
  },
  {
   "cell_type": "markdown",
   "id": "ee35c853",
   "metadata": {},
   "source": [
    "### _Creating an Interactive Recommendation Widget_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 192,
   "id": "e1e03d25",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.jupyter.widget-view+json": {
       "model_id": "7b7e632561834e3dafba0da22d61c13f",
       "version_major": 2,
       "version_minor": 0
      },
      "text/plain": [
       "Text(value='Toy Story', description='Movie Title:')"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "application/vnd.jupyter.widget-view+json": {
       "model_id": "15ebb0493d50406eb03a058dc8bc4c27",
       "version_major": 2,
       "version_minor": 0
      },
      "text/plain": [
       "Output()"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "movie_name_input = widgets.Text(\n",
    "    value=\"Toy Story\",\n",
    "    description=\"Movie Title:\",\n",
    "    disable = False\n",
    ")\n",
    "\n",
    "recommendation_list = widgets.Output()\n",
    "\n",
    "def on_type(data):\n",
    "    with recommendation_list:\n",
    "        recommendation_list.clear_output()\n",
    "        title = data[\"new\"]\n",
    "        if len(title) > 5:\n",
    "            results = search(title)\n",
    "            movie_id = results.iloc[0][\"movieId\"]\n",
    "            display(find_similar_movies(movie_id))\n",
    "        \n",
    "movie_name_input.observe(on_type, names=\"value\")\n",
    "\n",
    "display(movie_name_input, recommendation_list)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "45665ec0",
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "eb8704c7",
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3 (ipykernel)",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.11.3"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}

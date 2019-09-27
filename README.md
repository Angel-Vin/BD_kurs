# BD_kurs

-- phpMyAdmin SQL Dump
-- version 3.5.1
-- http://www.phpmyadmin.net
--
-- Хост: 127.0.0.1
-- Время создания: Сен 18 2019 г., 20:16
-- Версия сервера: 5.5.25
-- Версия PHP: 5.3.13

SET SQL_MODE="NO_AUTO_VALUE_ON_ZERO";
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8 */;

--
-- База данных: `task27`
--

-- --------------------------------------------------------

--
-- Структура таблицы `car`
--

CREATE TABLE IF NOT EXISTS `car` (
  `car` int(11) NOT NULL,
  `series` char(10) NOT NULL,
  `dateOfIssue` year(4) NOT NULL,
  `model` int(11) NOT NULL,
  `fuelType` int(11) NOT NULL,
  PRIMARY KEY (`car`),
  UNIQUE KEY `model` (`model`),
  UNIQUE KEY `fuelType` (`fuelType`)
) ENGINE=InnoDB DEFAULT CHARSET=ascii;

-- --------------------------------------------------------

--
-- Структура таблицы `carbody`
--

CREATE TABLE IF NOT EXISTS `carbody` (
  `id` int(11) NOT NULL,
  `nameBody` varchar(10) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=ascii;

-- --------------------------------------------------------

--
-- Структура таблицы `characteristics`
--

CREATE TABLE IF NOT EXISTS `characteristics` (
  `id` int(11) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=ascii;

-- --------------------------------------------------------

--
-- Структура таблицы `color`
--

CREATE TABLE IF NOT EXISTS `color` (
  `id` int(11) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=ascii;

-- --------------------------------------------------------

--
-- Структура таблицы `contract`
--

CREATE TABLE IF NOT EXISTS `contract` (
  `id` int(11) NOT NULL,
  `customer` int(11) NOT NULL,
  `dateOfConclusion` datetime NOT NULL,
  `dateOfDelivery` datetime NOT NULL,
  `grandTotal` float NOT NULL,
  `prepayment` float NOT NULL,
  `conditions` float NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `customer` (`customer`)
) ENGINE=InnoDB DEFAULT CHARSET=ascii;

-- --------------------------------------------------------

--
-- Структура таблицы `contractentries`
--

CREATE TABLE IF NOT EXISTS `contractentries` (
  `amount` int(2) NOT NULL,
  `id` int(11) NOT NULL,
  `characteristics` int(11) NOT NULL,
  `car` int(11) NOT NULL,
  `contract` int(11) NOT NULL,
  `employee` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `characteristics` (`characteristics`),
  UNIQUE KEY `car` (`car`),
  UNIQUE KEY `contract` (`contract`),
  UNIQUE KEY `contract_2` (`contract`),
  UNIQUE KEY `employee` (`employee`)
) ENGINE=InnoDB DEFAULT CHARSET=ascii;

-- --------------------------------------------------------

--
-- Структура таблицы `customer`
--

CREATE TABLE IF NOT EXISTS `customer` (
  `id` int(11) NOT NULL,
  `surname` varchar(15) NOT NULL,
  `name` varchar(11) NOT NULL,
  `patronymic` varchar(15) NOT NULL,
  `passport` int(7) NOT NULL,
  `discount` int(2) NOT NULL,
  `contractentries` int(11) NOT NULL,
  `constant` tinyint(1) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `contractentries` (`contractentries`),
  UNIQUE KEY `contractentries_2` (`contractentries`)
) ENGINE=InnoDB DEFAULT CHARSET=ascii;

-- --------------------------------------------------------

--
-- Структура таблицы `employees`
--

CREATE TABLE IF NOT EXISTS `employees` (
  `id` int(11) NOT NULL,
  `surname` varchar(15) NOT NULL,
  `name` varchar(11) NOT NULL,
  `patronymic` varchar(15) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=ascii;

-- --------------------------------------------------------

--
-- Структура таблицы `fueltype`
--

CREATE TABLE IF NOT EXISTS `fueltype` (
  `id` int(11) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=ascii;

-- --------------------------------------------------------

--
-- Структура таблицы `mark`
--

CREATE TABLE IF NOT EXISTS `mark` (
  `id` int(11) NOT NULL,
  `nameMark` varchar(10) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=ascii;

-- --------------------------------------------------------

--
-- Структура таблицы `model`
--

CREATE TABLE IF NOT EXISTS `model` (
  `id` int(11) NOT NULL,
  `mark` int(11) NOT NULL,
  `carbody` int(11) NOT NULL,
  `color` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `mark` (`mark`),
  UNIQUE KEY `carbody` (`carbody`),
  UNIQUE KEY `color` (`color`)
) ENGINE=InnoDB DEFAULT CHARSET=ascii;

-- --------------------------------------------------------

--
-- Структура таблицы `result`
--

CREATE TABLE IF NOT EXISTS `result` (
  `contract` int(11) NOT NULL,
  `contractentries` int(11) NOT NULL,
  `id` int(11) NOT NULL,
  `employee` int(2) NOT NULL,
  `factoryNumber` varchar(17) NOT NULL,
  `resultSum` float NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `contractentries` (`contractentries`),
  UNIQUE KEY `contract` (`contract`)
) ENGINE=InnoDB DEFAULT CHARSET=ascii;

--
-- Ограничения внешнего ключа сохраненных таблиц
--

--
-- Ограничения внешнего ключа таблицы `car`
--
ALTER TABLE `car`
  ADD CONSTRAINT `car_ibfk_2` FOREIGN KEY (`model`) REFERENCES `model` (`id`),
  ADD CONSTRAINT `car_ibfk_3` FOREIGN KEY (`fuelType`) REFERENCES `fueltype` (`id`);

--
-- Ограничения внешнего ключа таблицы `characteristics`
--
ALTER TABLE `characteristics`
  ADD CONSTRAINT `characteristics_ibfk_1` FOREIGN KEY (`id`) REFERENCES `contractentries` (`id`);

--
-- Ограничения внешнего ключа таблицы `contract`
--
ALTER TABLE `contract`
  ADD CONSTRAINT `contract_ibfk_1` FOREIGN KEY (`customer`) REFERENCES `customer` (`id`);

--
-- Ограничения внешнего ключа таблицы `contractentries`
--
ALTER TABLE `contractentries`
  ADD CONSTRAINT `contractentries_ibfk_1` FOREIGN KEY (`contract`) REFERENCES `contract` (`id`),
  ADD CONSTRAINT `contractentries_ibfk_2` FOREIGN KEY (`car`) REFERENCES `car` (`car`);

--
-- Ограничения внешнего ключа таблицы `employees`
--
ALTER TABLE `employees`
  ADD CONSTRAINT `employees_ibfk_1` FOREIGN KEY (`id`) REFERENCES `contractentries` (`employee`);

--
-- Ограничения внешнего ключа таблицы `model`
--
ALTER TABLE `model`
  ADD CONSTRAINT `model_ibfk_3` FOREIGN KEY (`color`) REFERENCES `color` (`id`),
  ADD CONSTRAINT `model_ibfk_1` FOREIGN KEY (`mark`) REFERENCES `mark` (`id`),
  ADD CONSTRAINT `model_ibfk_2` FOREIGN KEY (`carbody`) REFERENCES `carbody` (`id`);

--
-- Ограничения внешнего ключа таблицы `result`
--
ALTER TABLE `result`
  ADD CONSTRAINT `result_ibfk_1` FOREIGN KEY (`contract`) REFERENCES `contractentries` (`id`);

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
